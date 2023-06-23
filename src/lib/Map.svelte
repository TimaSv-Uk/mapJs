<script lang="ts">
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
  import Point from 'ol/geom/Point.js';
  import Select from 'ol/interaction/Select.js';
  import Stamen from 'ol/source/Stamen.js';
  import VectorSource from 'ol/source/Vector.js';
  import View from 'ol/View.js';
  import {Icon, Style} from 'ol/style.js';
  import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer.js';
  
  let map = null;
  const setupMap = (node:any) => {
  function createStyle(src, img) {
    return new Style({
      image: new Icon({
        anchor: [0.5, 0.96],
        crossOrigin: 'anonymous',
        src: src,
        img: img,
        imgSize: img ? [img.width, img.height] : undefined,
      }),
    });
  }
  
  const iconFeature = new Feature(new Point([0, 0]));
  iconFeature.set('style', createStyle('/vite.svg', undefined));
  
  const map = new Map({
    layers: [
      new TileLayer({
        source: new Stamen({layer: 'watercolor'}),
      }),
      new VectorLayer({
        style: function (feature) {
          return feature.get('style');
        },
        source: new VectorSource({features: [iconFeature]}),
      }),
    ],
    target: document.getElementById('map'),
    view: new View({
      center: [0, 0],
      zoom: 3,
    }),
  });
  
  // const selectStyle = {};
  // const select = new Select({
  //   style: function (feature) {
  //     const image = feature.get('style').getImage().getImage();
  //     if (!selectStyle[image.src]) {
  //       const canvas = document.createElement('canvas');
  //       const context = canvas.getContext('2d');
  //       canvas.width = image.width;
  //       canvas.height = image.height;
  //       context.drawImage(image, 0, 0, image.width, image.height);
  //       const imageData = context.getImageData(0, 0, canvas.width, canvas.height);
  //       const data = imageData.data;
  //       for (let i = 0, ii = data.length; i < ii; i = i + (i % 4 == 2 ? 2 : 1)) {
  //         data[i] = 255 - data[i];
  //       }
  //       context.putImageData(imageData, 0, 0);
  //       selectStyle[image.src] = createStyle(undefined, canvas);
  //     }
  //     return selectStyle[image.src];
  //   },
  // });
  // map.addInteraction(select);
  
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
  <img src="/vite.svg" alt="">
  <div id="map" class="map"  use:setupMap/>
</div>

<style>
  .map {
    width: 1200px;
    height: 800px;
  }
</style>
