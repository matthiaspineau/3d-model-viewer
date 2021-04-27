<script>
import { OBJLoader } from 'three/examples/jsm/loaders/OBJLoader';

import { toIndexed } from './util.js';
import mixin from './ModelMixins.vue';

export default {
  name: 'model-obj',
  mixins: [mixin],
  props: {
    lights: {
      type: Array,
      default() {
        return [
          {
            type: 'HemisphereLight',
            position: { x: 0, y: 1, z: 0 },
            skyColor: 0xaaaaff,
            groundColor: 0x806060,
            intensity: 0.2,
          },
          {
            type: 'DirectionalLight',
            position: { x: 1, y: 1, z: 1 },
            color: 0xffffff,
            intensity: 0.8,
          },
        ];
      },
    },
    smoothing: {
      type: Boolean,
      default: false,
    },
    mtlPath: {
      type: String,
    },
    mtl: {
      type: String,
    },
  },
  data() {
    return {
      loader: new OBJLoader()
    };
  },
  methods: {
    process(object) {
      if (this.smoothing) {
        object.traverse((child) => {
          if (child.geometry) {
            child.geometry = toIndexed(child.geometry);
            child.geometry.computeVertexNormals();
          }
        });
      }
    },
    
  },
};
</script>