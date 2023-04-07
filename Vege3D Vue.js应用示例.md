## Vege3D Vue.js应用示例

#### 1. 通过Vue CLI工具创建Vue.js应用
`npx @vue/cli create vue-app-example`

#### 2. 将以下文件从Verge3D发行版复制到应用中：
* 复制Verge3D目录 __manager/template/Embeddable/public__ 中的内容到 **vue-app-example/public**
* 复制Verge3D目录 __manager/templates/Embeddable/src__ 中的内容到 __vue-app-example/src__
* 复制Verge3D引擎文件 __build/v3d.js__ 到 __vue-app-example/public__

或使用PowerShell命令：

将 __V3D_PATH__ 和 __MY_PATH__ 分别更改为Verge3D发行版和 __Vue__ 应用所在的位置。

```
$V3D_PATH = "path\to\v3d\distribution"
$MY_PATH = "path\to\my\vue\app"
Copy-Item -Path "$V3D_PATH\manager\templates\Embeddable\public\*" -Destination "$MY_PATH\public" -Recurse
Copy-Item -Path "$V3D_PATH\manager\templates\Embeddable\src\*" -Destination "$MY_PATH\src" -Recurse
Copy-Item "$V3D_PATH\build\v3d.js" -Destination "$MY_PATH\public"
```

#### 3. 添加如下脚本标签至 __vue-app-example/public/index.html:__
`<script src="<%= BASE_URL %>v3d.js"></script>`

#### 4. 创建一个文件__vue-app-example/src/components/V3DApp.vue__ ,包含以下代码：
```
<template>
    <div :id="containerId">
      <div
        :id="fsButtonId"
        class="fullscreen-button fullscreen-open"
        title="Toggle fullscreen mode"
      ></div>
    </div>
  </template>
  
  <script>
  import { createApp } from '../v3dApp/app';
  
  export default {
    name: 'V3DApp',
  
    created() {
      this.app = null;
      this.PL = null,
  
      this.uuid = window.crypto.randomUUID();
      this.containerId = `v3d-container-${this.uuid}`;
      this.fsButtonId = `fullscreen-button-${this.uuid}`;
      this.sceneURL = 'v3dApp/app.gltf';
      this.logicURL = 'v3dApp/visual_logic.js';
  
      this.loadApp = async function() {
        ({ app: this.app, PL: this.PL } = await createApp({
          containerId: this.containerId,
          fsButtonId: this.fsButtonId,
          sceneURL: this.sceneURL,
          logicURL: this.logicURL,
        }));
      }
  
      this.disposeApp = function() {
        this.app?.dispose();
        this.app = null;
  
        // dispose Puzzles' visual logic
        this.PL?.dispose();
        this.PL = null;
      }
  
      this.reloadApp = function() {
        this.disposeApp();
        this.loadApp();
      }
    },
  
    mounted() {
      this.loadApp();
    },
  
    beforeDestroy() {
      this.disposeApp();
    },
  }
  </script>
  
  <style>
  @import '../v3dApp/app.css';
  </style><template>
    <div :id="containerId">
      <div
        :id="fsButtonId"
        class="fullscreen-button fullscreen-open"
        title="Toggle fullscreen mode"
      ></div>
    </div>
  </template>
  
  <script>
  import { createApp } from '../v3dApp/app';
  
  export default {
    name: 'V3DApp',
  
    created() {
      this.app = null;
      this.PL = null,
  
      this.uuid = window.crypto.randomUUID();
      this.containerId = `v3d-container-${this.uuid}`;
      this.fsButtonId = `fullscreen-button-${this.uuid}`;
      this.sceneURL = 'v3dApp/app.gltf';
      this.logicURL = 'v3dApp/visual_logic.js';
  
      this.loadApp = async function() {
        ({ app: this.app, PL: this.PL } = await createApp({
          containerId: this.containerId,
          fsButtonId: this.fsButtonId,
          sceneURL: this.sceneURL,
          logicURL: this.logicURL,
        }));
      }
  
      this.disposeApp = function() {
        this.app?.dispose();
        this.app = null;
  
        // dispose Puzzles' visual logic
        this.PL?.dispose();
        this.PL = null;
      }
  
      this.reloadApp = function() {
        this.disposeApp();
        this.loadApp();
      }
    },
  
    mounted() {
      this.loadApp();
    },
  
    beforeUnmount() {
      this.disposeApp();
    },
  }
  </script>
  
  <style>
  @import '../v3dApp/app.css';
  </style>
```

#### 5. 将 __vue-app-example/src/App.vue__ 中的内容替换为如下代码：
```
<template>
  <V3DApp></V3DApp>
</template>

<script>
import V3DApp from './components/V3DApp.vue';

export default {
  name: 'App',
  components: {
    V3DApp,
  },
}
</script>
```

#### 6. 通过在 __vue-app-example__ 目录中执行以下命令来运行开发服务器：
`npm run serve`

默认情况下，应用现在应该可以在 http://localhost:8080/ 地址中访问了。
