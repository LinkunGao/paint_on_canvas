# paint_on_canvas

- vue

- component use

  in the page that you want to use the component

  - import it

  ```ts
  import CanvasPaint from "./components/CanvasPaint.vue";
  ```

  - use it in template directly

  ```html
  <template>
    <CanvasPaint />
  </template>
  ```

- ref with v-if

  ```ts
  import { ref } from "vue";
  let mode = ref("mode1");
  ```

  - in `template`

  ```html
  <CanvasPaint v-if="mode === 'mode1'" />
  ```

- use asset img in component's style tag

  - use relative path directly

    ```css
    canvas {
      position: absolute;
      background-color: #f0f0f0;
      cursor: url("../assets/img/pencil-black.svg"), auto;
    }
    ```
