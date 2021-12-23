# Vue2 Vue3 比較筆記

## 開發環境建置

### Vue2 + vue-cli3
``` bash
npm install -g @vue/cli
npm install --global yarn
vue create [project name]
```

### Vue3 + Vite2
```
npm install -g create-vite-app
create-vite-app [project name]
```


## 生態系

### Vue2
* 編譯工具: [vue-cli 3](https://github.com/vuejs/vue-cli)
* 路由: [vue-router](https://github.com/vuejs/vue-router)
* 狀態管理: [vuex](https://github.com/vuejs/vuex)
* VSCodet插件: [Vuter](https://github.com/vuejs/vetur)


### Vue3
> 直至2021年底生態圈的推薦

* 編譯工具: [Vite2](https://github.com/vitejs/vite) + TypeScript
* 路由: [vue-router-next](https://github.com/vuejs/vue-router-next)
* 狀態管理: [Pinia](https://github.com/vuejs/pinia)
* VSCodet插件: [Volar](https://github.com/johnsoncodehk/volar)


### 名詞解釋

SFC : .vue 結尾的文件，使用export default 輸出組件。
通常包含三種類型的頂級語言塊 
- **template** HTML 
- **script** JS 
- **style**  CSS 

Options API:

在vue2中，我們會在一個vue檔案中methods，computed，watch，data中等等定義屬性和方法，共同處理頁面邏輯，我們稱這種方式為Options API
缺點：一個功能往往需要在不同的vue配置項中定義屬性和方法，比較分散，專案小還好，清晰明瞭，但是專案大了後，一個methods中可能包含很多個方法，你往往分不清哪個方法對應著哪個功能



Composition API:
在 Vue3 中仍然可使用 Vue2 編寫組件的方法，其好處如下：
提供更好的 TypeScript 支持
組件太大，需要依功能分類時可使用
需要跨組件使用程式碼



---




### Vue Composition API vs React Hook

``` js
<template>
  <div>
    <span>{{count}}</span>
    <button @click="add"> +1 </button>
  </div>
</template>

export default {
    setup() {
        const count = ref(0)

        const add = () => count.value++

        effect(function log(){
            console.log('count changed!', count.value)
        })

        return { count, add }
    }
}

```


``` js

export default function Counter() {
  const [count, setCount] = useState(0);

  const add = () => setCount((prev) => prev + 1);

  useEffect(() => {
    console.log("count updated!", count);
  }, [count]);

  return (
    <div>
      <span>{count}</span>
      <button onClick={add}> +1 </button>
    </div>
  );
}


```