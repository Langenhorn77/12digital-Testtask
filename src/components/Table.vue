<template>
    <div class="wrapper">
        <div class="user-input">
            <label for="num">Введите число</label>
            <input id="num" type="number" min="1" max="1000" ref="input" @input="onInput"/>
            <span>{{errMessage}}</span>
            <button type="button" @click="validate">Рассчитать</button>
        </div>
        
        <ul class="data">
            <li class="data__item" v-for="item in list" :key="item.id">
                <p>{{item}}</p>
            </li>
        </ul>
        
        <div class="result" v-show="valid">
            {{this.result}}
        </div>
    
    
    </div>
</template>

<script>
    export default {
        name: 'Table',
        data() {
            return {
                list: [],

                countObj: [
                    {
                        count: 0,
                        name: 'Карликовый'
                    },

                    {
                        count: 0,
                        name: 'Индриевый'
                    },

                    {
                        count: 0,
                        name: 'Руконожковый'
                    }
                ],

                errMessage: '',

                result: '',
                resultNumber: 0,
                valid: false
            }
        },


        methods: {
            getRandomIndex(min, max) {
                return min + Math.floor(Math.random() * (max + 1 - min));
            },

            fillArray(n) {
                this.valid = true;
                for (let i = 1; i <= n; i++) {
                    let indexValue = this.getRandomIndex(0, this.countObj.length - 1);
                    if (n < 2) {
                        this.list.push(this.countObj[indexValue].name);
                    } else {
                        if (i < n) {
                            this.countObj[indexValue].count++;
                            this.list.push(this.countObj[indexValue].name)
                        } else {
                            this.resultNumber = this.countObj.reduce((acc, curr, i) => this.countObj[acc].count > curr.count ? acc : i, 0);
                            this.list.push(this.countObj[this.resultNumber].name)
                        }
                    }

                }
                this.result = this.list.sort((a, b) => a.count < b.count ? 1 : -1)[0];
            },

            onInput() {
                this.list = [];
                this.valid = false;
                this.errMessage = '';
                this.result = '';
                for (let j = 0; j < this.countObj.length; j++) {
                    this.countObj[j].count = 0;
                }
            },

            validate() {
                let num = this.$refs.input.value;
                if (!num) {
                    this.errMessage = 'Введите число!'
                } else if (num < 1) {
                    this.errMessage = 'Введите число больше 0!'
                } else if (num > 1000) {
                    this.errMessage = 'Введите число не больше 1000!'
                } else {
                    this.onInput();
                    this.fillArray(num);
                }
            }
        },

    }
</script>


<style scoped>
    .wrapper {
        width: 320px;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
        margin-left: auto;
        margin-right: auto;
    }
    
    .user-input {
        width: 100%;
        height: 85px;
        display: flex;
        flex-direction: column;
        flex-wrap: wrap;
        justify-content: space-between;
        margin-bottom: 45px;
        position: relative;
    }
    
    .user-input label {
        font-size: 20px;
        font-weight: 700;
        text-align: left;
    }
    
    .user-input input {
        width: 150px;
        height: 35px;
        box-sizing: border-box;
        border-radius: 3px;
        border: 2px solid blue;
    }
    
    .user-input button {
        width: 150px;
        height: 35px;
        box-sizing: border-box;
        border-radius: 3px;
        border: 2px solid blue;
        background-color: transparent;
        align-self: flex-end;
        margin-top: 50px;
        font-size: 16px;
    }
    
    .user-input button:disabled {
        opacity: 0.3;
    }
    
    .user-input span {
        font-size: 10px;
        color: red;
        text-align: left;
        position: absolute;
        bottom: -20px;
        left: 0;
    }
    
    
    .data {
        width: 150px;
        list-style: none;
        padding: 0;
        margin: 0;
    }
    
    .data__item {
        box-sizing: border-box;
        height: 55px;
        border: 1px solid black;
        margin-bottom: 2px;
    }
    
    .result {
        width: 150px;
        border: 1px solid red;
        box-sizing: border-box;
        height: 55px;
        padding-top: 16px;
    }

</style>
