# Vue 国际化配置

1. 布置好 `vue` 项目 `vue create test`
2. 进入 `vue` 项目 `cd test`
3. 执行 `npm install  vue-i18n -S` 添加国际化组件
4. 添加如下配置

    ```js
    import Vue from 'vue'
    import VueI18n from 'vue-i18n'
    Vue.use(VueI18n)
    const i18n = new VueI18n({
        locale: localStorage.getItem('locale') || 'en',
        messages: {
            en: {
                name: 'name',
                page: 'page {page}',
            },
            zh: {
                name: '姓名'
                page: '页面 {page}'
            }
        }
    })
    
    new Vue({
        i18n,
        render: h => h(App)
    }).$mount('#app')

    ```

5. 以上配置可以通过文件组织
    + 文件树形图

        ```js
        // i18n
        // ├── config
        // │   ├── en.js
        // │   └── zh.js
        // └── index.js
        ```

    + 各个文件内容

        ```js
        // index.js
        import Vue from 'vue'
        import zh from './config/zh'
        import en from './config/en'
        import VueI18n from 'vue-i18n'
        Vue.use(VueI18n)
        const i18n: VueI18n = new VueI18n({
            locale: localStorage.getItem('locale') || 'en',
            messages: {
                en,
                zh
            }
        })
        export default i18n
        
        // en.js
        export default {
            name: 'name',
            page: 'page {page}'
        }
        // zh.js
        export default {
            name: '名字',
            page: '页面 {page}'
        }
        ```

    + main.js 添加内容

        ```js
        import i18n from './i18n'
        new Vue({
            i18n,
            render: h => h(App)
        }).$mount('#app')
        ```

6. 使用方式

    ```js
    // 没有参数
    {{$t('name')}}
    // 有参数
    {{$t('page', {page: 1})}}
    ```
