<template>
  <div id="flow">
    <div class="bx-grid has-top">
      <div class="bx-grid-left">
        <h1 class="first-header h4">Menu</h1>
        <hr>Menu here
      </div>
      <div class="bx-grid-middle">
        <h2 class="first-header h4">
          <router-link :to="`/`">Flow</router-link>
        </h2>
        <hr>
        <div class="pages" id="myFlow">
          <kendo-hierarchicaldatasource 
            ref="remoteDataSourceComponent" 
            :data="diagramData" 
            schema-model-children="items">
          </kendo-hierarchicaldatasource>

          <kendo-diagram 
            data-source-ref="remoteDataSourceComponent" 
            layout-type="layered" 
            :shape-defaults-visual="visualTemplate"
            connection-defaults-stroke-color="#979797" 
            :connection-defaults-stroke-width="2" 
            @kendowidgetready="kendowidgetready">
          </kendo-diagram>
        </div>
      </div>
      <div class="bx-grid-right" v-if="debug">
      diagramData,
        <code>{{flowData}}</code>
      </div>
    </div>
  </div>
</template>
  
<script>
// import { mapActions } from 'vuex'
import Vue from 'vue';
import '@progress/kendo-ui';
import '@progress/kendo-theme-default/dist/all.css';
import { DataSourceInstaller } from '@progress/kendo-datasource-vue-wrapper';
import { DiagramInstaller } from '@progress/kendo-diagram-vue-wrapper';

import diagramData from './data';

Vue.use(DiagramInstaller);
Vue.use(DataSourceInstaller);

export default {
  name: "flow",
  data() {
    return {
      debug: false,
      diagramModel: {
        children: 'items',
      },
      diagramData,
    }
  },
  methods: {
    kendowidgetready(widget) {
      widget.bringIntoView(widget.shapes);
    },
    visualTemplate(options) {
      var dataviz = kendo.dataviz;
      var g = new dataviz.diagram.Group();
      var dataItem = options.dataItem;

      g.append(new dataviz.diagram.Rectangle({
        width: 210,
        height: 75,
        stroke: {
          width: 0,
        },
        fill: {
          gradient: {
            type: 'linear',
            stops: [{
              color: dataItem.colorScheme,
              offset: 0,
              opacity: 0.5,
            }, {
              color: dataItem.colorScheme,
              offset: 1,
              opacity: 1,
            }],
          },
        },
      }));

      g.append(new dataviz.diagram.TextBlock({
        text: dataItem.firstName + ' ' + dataItem.lastName,
        x: 85,
        y: 20,
        fill: '#fff',
      }));

      g.append(new dataviz.diagram.TextBlock({
        text: dataItem.title,
        x: 85,
        y: 40,
        fill: '#fff',
      }));

      g.append(new dataviz.diagram.Image({
        source: 'https://demos.telerik.com/kendo-ui/content/dataviz/diagram/people/' + dataItem.image,
        x: 3,
        y: 3,
        width: 68,
        height: 68,
      }));

      return g;
    }
  }
}
</script>


<style lang="scss" scoped>
</style>
  