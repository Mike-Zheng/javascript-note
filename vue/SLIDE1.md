---
title: vue3 初上        # 簡報的名稱
tags: presentation   # 簡報的標籤
slideOptions:        # 簡報相關的設定
  theme: Vim   # 顏色主題
  transition: 'fade' # 換頁動畫
	allottedMinutes: 5  # 預計一張簡報花幾分鐘
	parallaxBackgroundImage: https://s3.amazonaws.com/hakim-static/reveal-js/reveal-parallax-1.jpg
---

# vue3 初上

## 學習使我快樂

![](https://i.imgur.com/DTjL52Q.png)




---

### OUTLINE

- 前端淺談
- 滿滿的TODO大平台
- vue3 + React
- 升級 所謂的生態系

---

#### 為什麼我們使用vue

![](https://i.imgur.com/Vrqy9Pa.png)



Note:
  vue大概是我2017年引進公司，15 16年我主要的工作項目其實都是使用angularJS，當初會引入vue的原因不外乎是前端工程化與legcy code的相容程度，當然還有一部分是因為他跟angular的pattern相似。
	基本上來說作為必須持續維護legcy code的系統，dc cmtweb ea等等，react的引進只會讓自己跳坑，沒有辦法partial update，要改的太多了只好改天了。
	而當時的vue跟jquery借鏡的就是cdn也能跑這個優勢，讓傳統code都可以局部慢慢的升級，而我們第一個使用上的就是opcua nodetree的前端化。

---

##### vue 和 react的活耀度

 npm 的數據跟 React 比維持在 1:4 左右的比例。

---

Chrome 開發者插件數據一周活動
- React: 在 3~4M 之間
- Vue: 1.92M（stable 1.7M + beta 0.22M）


純web 端的占有比例在 1:2 ～ 1:1.5 之間。


[知呼:尤雨溪](https://www.zhihu.com/question/472193255/answer/2235015723)

Note:
  react 的 npm 數據有相當部分來自 React Native，
純 web 端的比較最靠譜的數據是看 Chrome 開發者插件數據的一周活動，

---

#### 點亮vue的項目

![](https://i.imgur.com/DPVXuim.png)



[知呼:尤雨溪](https://www.zhihu.com/question/472193255/answer/2235015723)



---

## 那 vue3香嗎? 





---

##### 滿滿的TODO大平台

![](https://i.imgur.com/mbpv3Qd.png)



---


##### SFC template


![](https://i.imgur.com/VQFctmB.jpg)



---


##### 我們熟知的vue2


![vue2 todo](https://i.imgur.com/qDeq8JF.jpg)




---

##### 折起來

![](https://i.imgur.com/7u1XJfu.png)




---



## 今晚 我想加點....

<!-- .slide: data-background="https://i.imgur.com/emk7QlC.png" -->


- 主管說 唉呦 不錯 加加這個
- 老闆說 歐呦 讚讚 換點這個
- 客戶說 欸欸 可以 還想要這個
- ...
- ...
- ...



---


![](https://i.imgur.com/Xdq92l2.png)


----

![](https://i.imgur.com/SXqsyqz.png)



----

![](https://i.imgur.com/YmVAgMm.png)






---

#### 那我想寫成這樣可以嗎


![](https://i.imgur.com/zL1LNLD.png)



----


## 我說那個mixin呢

vue2 其實可以這樣寫

##### code寫不好就容易散、並且不便於維護、新手難以理解

- mixins可以加任何代碼，props、data、methods、各種東西，就導致如果不了解mixins封裝的代碼今天你不知道其他使用的狀況下多加一個data內的變數，都會導致正在使用的組件出問題





---


 Vue2 目前的問題

- 缺少乾淨的在多個組件之間提取和復用邏輯的機制。
- 類型推斷不夠友好。

note:
   首先拋出 Vue2 的代碼模式下存在的幾個問題。隨著功能的增長，復雜組件的代碼變得越來越難以維護。 尤其發生妳去新接手別人的代碼時。 根本原因是 Vue 的現有 API 通過「選項」組織代碼，但是在大部分情況下，通過邏輯考慮來組織代碼更有意義。
- 缺少乾淨的在多個組件之間提取和復用邏輯的機制。
- 類型推斷不夠友好。






---

##### vue 3 Composition API


![](https://i.imgur.com/EhZtYbc.jpg)



----

##### 我們熟知的vue2


![vue2 todo](https://i.imgur.com/qDeq8JF.jpg)





---






##### 結構比較

![](https://i.imgur.com/gXmPGCj.png)


---

##### 甚至你還可以

![](https://i.imgur.com/ojzVqcJ.png)


---

##### 甚至你還可以 code

![](https://i.imgur.com/0ynfjbA.jpg)

----

##### vue 3 Composition API


![](https://i.imgur.com/EhZtYbc.jpg)


---

##### 另外打包 compostion/Todo

![](https://i.imgur.com/rg9lCht.jpg)


---

#### 還能更精簡

![](https://i.imgur.com/3iTUCWh.jpg)


---



#### 名詞解釋

SFC : .vue 結尾的文件
- `template` -> HTML 
- `script` -> JS 
- `style` -> CSS 

Composition API: vue2 結構

Options API: vue3 新結構


note: 
  .vue 結尾的文件，使用export default 輸出組件。
通常包含三種類型的頂級語言塊 


---



### 為什麼Vue3加入了Composition API？

1. 提高對TypeScript更好的支援度。
2. 增加元件的易維護性。
3. 讓功能被拆分出來並能重複利用。


note: 
  在Vue3中，依然可以使用Options API，但為了以下幾個部分，所以在Vue3加入了Compositon API。


---

### 第三主題 React vs Vue


---

Vue Composition API

![](https://i.imgur.com/gDKMjX1.jpg)



---

React hook

![](https://i.imgur.com/1Hrcur2.jpg)


---

![](https://i.imgur.com/HNo5jqN.png)



---

### 那生態系呢

---


### Vue2
* 編譯工具: [vue-cli 3](https://github.com/vuejs/vue-cli)
* 路由: [vue-router](https://github.com/vuejs/vue-router)
* 狀態管理: [vuex](https://github.com/vuejs/vuex)
* VSCodet插件: [Vuter](https://github.com/vuejs/vetur)



---

### Vue3
> 直至2021年底生態圈的推薦

* 編譯工具: [Vite2](https://github.com/vitejs/vite) + TypeScript
* 路由: [vue-router-next](https://github.com/vuejs/vue-router-next)
* 狀態管理: [Pinia](https://github.com/vuejs/pinia)
* VSCodet插件: [Volar](https://github.com/johnsoncodehk/volar)


---


## 為什麼要typescript




---


![](https://i.imgur.com/kENfIJn.png)
### 2018(vue2)

---

## 其實這份投影片還沒準備ts部分





---

### 結論

- vue3 真香
- 想升級? 讓子彈飛一飛
- vue(3)+ts 拖稿一下


