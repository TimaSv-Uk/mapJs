<script lang="js">
  //   import { Feature, Map } from "ol";
  //   import TileLayer from "ol/layer/Tile";
  //   import "ol/ol.css";
  //   import { fromLonLat } from "ol/proj";
  //   import VectorLayer from "ol/layer/Vector";
  //   import VectorSource from "ol/source/Vector";
  //   import Style from "ol/style/Style";
  //   import Point from "ol/geom/Point";
  //   import Icon from 'ol/style/Icon';
  // // Svelte
  //   import View from "ol/View";
  //   import OSM from "ol/source/OSM";
  
  
  //   let img = "assets/svelte.svg"
  // // Exports
  // let mapId = 20;
  // // Local state
  // let map = null;
  
  // // functions
  // const setupMap = (node:any) => {
  //   function createStyle(src:any, img:any) {
  //   return new Style({
  //     image: new Icon({
  //       anchor: [0.5, 0.96],
  //       crossOrigin: 'anonymous',
  //       src: src,
  //       img: img,
  //       imgSize: img ? [img.width, img.height] : undefined,
  //     }),
  //   });
  // }
  // const iconFeature = new Feature(new Point(fromLonLat([30.523333, 50.450001])));
  //   // const iconFeature = new Feature({
  //   //   geometry: new Point(),
  //   //   name: "Kiyv",
  //   // });
  
  //   iconFeature.set('style', createStyle('/vite.svg', undefined));
  //   const vectorSource = new VectorSource({
  //     features: [iconFeature],
  //   });
  
  //   const vectorLayer = new VectorLayer({
  //     source: vectorSource,
  //   });
  
  //   const osmLayer = new TileLayer({
  //     source: new OSM(),
  //   });
  //   map = new Map({
  //     target: node.id,
  //     layers: [osmLayer, vectorLayer],
  //     view: new View({
  //       center: fromLonLat([30.523333, 50.450001]),
  //       zoom: 13,
  //     }),
  //   });
  //   return {
  //     destroy() {
  //       if (map) {
  //         // as Map
  //         map.setTarget(null);
  //         map = null;
  //       }
  //     },
  //   };
  // };
  import Feature from 'ol/Feature.js';
  import Map from 'ol/Map.js';
  import { fromLonLat } from "ol/proj";
  import Point from 'ol/geom/Point.js';
  import Select from 'ol/interaction/Select.js';
  import Stamen from 'ol/source/Stamen.js';
  import VectorSource from 'ol/source/Vector.js';
  import View from 'ol/View.js';
  import {Icon, Style} from 'ol/style.js';
  import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer.js';
  import OSM from "ol/source/OSM"
  import {defaults} from 'ol/control.js';

  let pointToggleHtml = false;
  let map = null;
  const setupMap = (node) => {
  function createStyle(src, img) {
    return new Style({
      image: new Icon({
        anchor: [0.5, 0.96],
        crossOrigin: 'anonymous',
        src: src,
        // img: img,
        // imgSize: img ? [img.width, img.height] : undefined,
      }),
    });
  }
  
  const iconFeature = new Feature(new Point(fromLonLat([30.523333, 50.450001])));
  iconFeature.set('style', createStyle('/vite.svg', undefined));

    // const iconFeature = new Feature({
    //   geometry: new Point(),
    //   name: "Kiyv",
    // });
  
    const vectorSource = new VectorLayer({
        style: function (feature) {
          return feature.get('style');
        },
        source: new VectorSource({features: [iconFeature]}),
      })

  const map = new Map({
    controls: defaults({zoom: false, attribution: false,rotate:false,}),
    layers: [
      // new TileLayer({
        // source: new Stamen({layer: 'terrain'}),
      // }),
      new TileLayer({
      source: new OSM(),
      }),
      vectorSource,
    ],
    target: document.getElementById('map'),
    view: new View({
      center: fromLonLat([30.523333, 50.450001]),
      zoom: 13,
      minZoom: 8,
      maxZoom: 18,
    }),
    
  });
  
  map.on('click', (event) => {
  const feature = map.forEachFeatureAtPixel(event.pixel, (feature) => feature);
  if (feature) {
    // Show or hide HTML content here
    pointToggleHtml = !pointToggleHtml
  }
});
  map.on('pointermove', function (evt) {
    map.getTargetElement().style.cursor = map.hasFeatureAtPixel(evt.pixel)
      ? 'pointer'
      : '';
  });
  
  return {
      destroy() {
        if (map) {
          // as Map
          map.setTarget(null);
          // map = null;
        }
      },
    };
  }
  </script>

<div>
  <div id="map" class="map"  use:setupMap/>
  {#if pointToggleHtml}
    <h1>Bob</h1>
  {/if}
</div>

<style>
  .map {
    width: 1100px;
    height: 500px;
  }

</style>
