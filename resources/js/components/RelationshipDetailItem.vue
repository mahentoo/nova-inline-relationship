<template>
  <div class="card shadow-md mb-4">
    <div class="bg-30 flex p-2 border-b border-40">
      <span>
        <button
          v-if="isCollapsed"
          class="btn btn-default btn-icon btn-white mr-3 p-1"
          @click="isCollapsed=false"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            width="24"
            height="24"
          ><path
            class="heroicon-ui"
            d="M17 11a1 1 0 0 1 0 2h-4v4a1 1 0 0 1-2 0v-4H7a1 1 0 0 1 0-2h4V7a1 1 0 0 1 2 0v4h4z"
          /></svg>
        </button>
        <button
          v-else
          class="btn btn-default btn-icon btn-white mr-3 p-1"
          @click="isCollapsed=true"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
            width="24"
            height="24"
          ><path
            class="heroicon-ui"
            d="M17 11a1 1 0 0 1 0 2H7a1 1 0 0 1 0-2h10z"
          /></svg>
        </button>
      </span>
      <span class="font-normal text-90 py-2 px-2">
        {{ label }} {{ id+1 }}
      </span>
    </div>
    <transition name="slide-fade">
      <div v-if="! isCollapsed">
        <div
          v-for="(parameter, attrib) in fields"
          :key="attrib"
          class="w-full px-6"
        >
          <component
            :is="'detail-' + parameter.meta.component"
            :field="parameter.meta"
            :resource-id="modelId"
            :resource-name="modelKey"
          />
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
    export default {
        name: "RelationshipDetailItem",

        props: {
            value: Object,
            settings: Object,
            collapsed:{
                type: Boolean,
                default: false
            },
            label: String,
            id: Number,
            modelId: Number,
            modelKey: String,
        },

        data: function (){
            return {
                isCollapsed : false
            }
        },

        computed: {
            fields: function (){
                let fields = {...this.value};
                Object.keys(fields).map(
                    attrib => {
                        fields[attrib].meta['name'] = fields[attrib].meta['singularLabel'];
                    }
                );
                return fields;
            }
        },

        methods: {
            getLabel: function (attrib){
                return this.getSettings(attrib, 'label') || attrib;
            },

            getSettings: function (attrib, key){
                return this.settings && this.settings.hasOwnProperty(attrib) && this.settings[attrib].hasOwnProperty(key) ? this.settings[attrib][key] : ''
            },
        },

        watch : {
            'collapsed' : function (){
                this.isCollapsed = this.collapsed;
            }
        }
    }
</script>
