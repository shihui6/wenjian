***vuex
    1-用法：
        1-1：安装vuex：npm install vuex --save
        1-2:在项目里创建vuex
            1-2-1：在src中新建一个store目录，在该目录中新建一个index.js
            1-2-2:在index.js中创建Vuex.Store实例保存到变量store中，最后使用export default导出
            1-2-3：在main.js中引入该文件,在vue实例全局引入store对象
            实例：
                ```js
                // store目录下的index.js文件
                    import Vue from 'vue'
                    import Vuex from 'Vuex'
                    //使用Vuex
                    Vue.use(Vuex)
                    // 创建vue实例
                    const store = new Vuex.store({

                    })
                    export default store  //导出store

                    // main.js文件
                    import store from './store'  //引入store
                    new Vue({
                        el:'#app',
                        store,      //将vue实例全局引入store
                        components:{App},
                        template:'<App/>'
                    })
                ```
        
        1-3：State
            1-3-1：概念：保存数据的地方
            1-3-2：用法：在全局通过this.$store.state来获取在state里面保存的数据(定义的数据)
            实例：
                ```js
                    import Vue from 'vue'
                    import Vuex from 'Vuex'
                    //使用Vuex
                    Vue.use(Vuex)
                    // 创建vue实例
                    const store = new Vuex.store({
                        state:{
                            count:1
                        }
                    })
                    export default store  //导出store
                ```
            
        1-4：Getters
            1-4-1：Getters相当于vue的computed属性
            1-4-2：运行机制：getter的返回值会根据他的依赖被缓存起来，且只有当他的依赖值发生改变才会被重新计算
            1-4-3：这里我们可以通过vuex的Getter来获取，Getters用于监听，state中的值的变化，返回计算的结果
            实例：
            ```js
            import Vue from 'vue'
            import Vuex from 'Vuex'
            //使用Vuex
            Vue.use(Vuex)
            // 创建vue实例
            const store = new Vuex.store({
                state:{
                    count:1
                },
                getters:{
                    // getters中的getStateCount方法接受一个参数state，这个参数就是我们用来保存数据的那个对象
                    getStateCount(state){   
                        return state.count + 1
                    }
                }
            })
            export default store  //导出store
            ```
            注意点：不会改变state.count本身的值

        1-5：Mutations
            1-5-1：概念：修改store中的值的唯一办法就是提交mutation来修改
            1-5-2：用法：在mutations中定义函数方法，用来对state里面的count修改值，在全局通过commit提交触发mutations里的方法
            实例：
            ```js
                // 在组件中
                methods:{
                    addFun(){
                        this.$store.commit('add')
                    },
                    reduction(){
                        this.$store.commit('reduction')
                    }
                }

                // 在vuex中
                const store = new Vuex.store({
                        state:{
                            count:1
                        },
                        mutations:{
                            add(state){   //mutations里的方法参数是state保存数据的对象
                                state.count = state.count + 1
                            },
                            reduction(state){
                                state.count = state.count - 1
                            }
                        }
                    })
                export default store
            ```

        1-6：Actions
            1-6-1：概念：虽然mutations达到了修改store状态值的目的，但是，官方不建议我们这样直接去修改store里面的值，而是让我们提交一个actions，在actions中提交mutation再去修改状态值
            1-6-2：用法：在vuex里注册一个actions(相当于vue里面methods)，然后全局使用dispatch来提交actions
            实例：
            ```js
                // 在vuex中
                const store = new Vuex.store({
                        state:{
                            count:1
                        },
                        mutations:{
                            add(state){   //mutations里的方法参数是state保存数据的对象
                                state.count = state.count + 1
                            },
                            reduction(state){
                                state.count = state.count - 1
                            }
                        },
                        actions:{
                            addFun(context,n){  //actions里面的方法接受的参数context是与store实例具有相同方法的对象
                                context.commit('add')  //通过提交commit触发mutation里面的add方法达到修改store状态的目的
                            },
                            reductionFun(context){
                                context.commit('reduction')
                            }
                        }
                    })
                export default store

                // 在组件中
                methods:{
                    addFun(){
                        this.$store.dispatch('addFun')  //调用store中的actions里面的addFun方法
                    },
                    reductionFun(){
                        this.$store.dispatch('reductionFun')
                    }
                }
            ```
                事例执行机制：组件中addFun方法调用store中的actions里面的addFun方法，然后在vuex中，通过actions里的addFun方法提交commit触发mutation里面的add方法达到修改store状态的目的
                注意点：dispatch中传入第二个参数

        1-7：mapState,mapGetters,mapActions
            1-7-1:概念：如果不喜欢使用this.$store.state.count和this.$store.dispatch('funName')这种很长的写法，那我们可以使用mapState,mapGetters,mapActions就不会这么麻烦了
            1-7-2：概念追加解释：mapState是state的辅助函数，也可以说成语法糖(内部有自己自定的定义)，mapState对应写在computed计算属性里面；
            实例：
            ```js
                // 在组件中
                <template>
                    <div>{{count1}}</div>
                </template>
                <script>
                    import {mapState,mapActions,mapGetters} from 'vuex'
                    export default {
                        computed:{
                            ...mapState({          //...形式是对象展开符
                                count1:state=>state.count   //引入了count1后，在页面中可以使用count1了
                            }),
                            ...mapGetters(['filteredList']) //使用对象展开符将getter混入computed
                        },
                        methods:{
                                // mapMutations使用方法一
                            ...mapMutations(['increase']),  //将this.increase()映射为this.$store.commit('increase')
                                // mapMutations使用方法二
                            ...mapMutations({
                                add:'increase'  ////将this.add()映射为this.$store.commit('increase')
                            }),
                            handIncrease(){
                                this.increase(5)  //调用mapMutation里的increase方法,可以直接在全局调用方法里面传参数
                            }
                        },
                    }
                </script>
            ```
                注意点：可在全局方法里；传参数