<template>
    <div>
        <!-- Step1  -->
        <div class="step1" v-if="step==1">
            <h1>Нам важно Ваше мнение!</h1>
            <p><b>Оцените пожалуйста Вашу готовность рекомендовать «Газпромнефть-Корпоративные продажи» своим коллегам / партнерам?</b></p>
            <p>Для оценки используйте 10-балльную шкалу, где 10 – точно готовы рекомендовать, 1 – точно не готовы рекомендовать.</p>         
            <div class="ranking">
                <img src="../assets/dislike-3-x.png" srcset="../assets/dislike-3-x@2x.png 2x,../assets/dislike-3-x@3x.png 3x"  :style="{opacity: 1.1-(rate/10)}">
                <div class="lables">
                    <label class="container_radio" v-for="number in 10" :key="number">
                        <span>{{number}} </span>
                        <input type="radio" :class="number <= rate ? 'checked': null" name="radio" :id="`id_${number}`" :value="number" v-model="rate">
                        <span class="checkmark_radio"></span>
                     </label>   
                    <div class="rankLine">
                        <div class="rankProgress" :style="{width:(rate-1)*73 + 'px'}"></div>
                    </div>
                </div>  
                <img src="../assets/like-3-x.png" :style="{opacity:rate/10}" srcset="../assets/like-3-x@2x.png 2x, ../assets/like-3-x@3x.png 3x"> 
            </div>
        </div>

        <!-- Step2  -->
        <div class="step2" v-if="step==2">
            <h1>Почему Вы не поставили оценку 10?</h1>
            <p>Выберите не более 3-х ключевых причин.</p> 
            <label class="container_checkbox" v-for="(reason,index) in reasons" :key="index">{{reason.label}}
                <input type="checkbox" :value="reason.id" v-model="pickedReason">
                <span class="checkmark_checkbox"></span>
            </label>
            <p v-if="showError" class="error">Выбирите хотя бы одну из причин</p>
        </div>

        <!-- Step3  -->
        <div class="step3" v-if="step==3">
            <h1>Багодарим за оценку!</h1>
            <p>Мы будем признательны, если Вы оставите отзыв или предложение по улучшению работы «Газпромнефть-Корпоративные продажи».</p> 
            <textarea name="message" id="message" v-model="message"
            placeholder="Ваш ответ..."></textarea>
        </div>

        <!-- Step4  -->
        <div class="step4" v-if="step==4">
            <h1>Спасибо!</h1>
        </div>

        <div class="btnsBlock" :style="[step == 4 ? {justifyContent: 'center', marginTop: '30px'} : null]">
            <button class="sendBtn" v-if="step != 4" @click="btnHandler">Отправить</button>
            <span v-if="step == 1" @click="close">Больше не спрашивать</span>
            <span v-if="step != 1" class="closeBtn" @click="close"
            >Закрыть</span>
        </div>
    </div>
</template>

<script>

import axios from 'axios';

export default {
    name: 'steps',
    data(){
        return{
            step:1,
            rate: 10,
            message:'',
            finish: false,
            showError: false,
            pickedReason:[],
            reasons: [
                {
                    "id": 9,
                    "label": "Задержки документооборота"
                },
                {
                    "id": 7,
                    "label": "Неудобный личный кабинет / не хватает опций"
                },
                {
                    "id": 3,
                    "label": "Не устраивает тарифная политика"
                },
                {
                    "id": 5,
                    "label": "Текущие рабочие вопросы решаются медленно"
                },
                {
                    "id": 4,
                    "label": "Неудобная отчетность / нет нужных отчетов"
                },
                {
                    "id": 6,
                    "label": "Неудобные / долгие процедуры заключения договора"
                },
                {
                    "id": 8,
                    "label": "Сбои при работе с личным кабинетом"
                },
                {
                    "id": 1,
                    "label": "Мало дополнительных сервисов для водителей по топливным картам"
                },
                {
                    "id": 2,
                    "label": "Не устраивает работа менеджеров"
                },
                {
                    "id": 10,
                    "label": "Другое"
                }
            ]
        }
    },

    watch: {
        pickedReason(newCheck, oldCheck) {
            if (newCheck.length!=0){
                this.showError = false
            }
        }
    },
    methods: {

    btnHandler() {
    if (this.step==1 && this.rate!=10){
            this.step++
        }
        else if (this.step == 1 && this.rate == 10 || this.step == 2 && this.pickedReason.length != 0){
        this.step = 3
        }
        else if (this.step == 2 && this.pickedReason.length == 0){
        this.showError = true
        }
        else if (this.step == 3){
            this.finish = true
            this.onSubmit()
            this.step = 4
        }
       },

        onSubmit() {
            const formData = {
                finish: this.finish,
                rate: this.rate,
                reasons: this.pickedReason,
                message:this.message
            }
            axios.post('https://vue-questionnaire-fffbb.firebaseio.com/answers.json',formData)
            .then(res=>console.log('success', res)
            )
            .catch(err => console.log('error', err));
        },

        close() {
            if(this.step == 2 || this.step == 3){
                this.finish = false
                this.onSubmit()
            }
            this.$emit('closeModal');
        }
    },
}
</script>


<style lang="scss" scoped>

$placeholderColor: #b3b3b3;
$cerulianColor: #335bb7;
$charcoalColor: #444;

h1 {
    height: 42px;
    margin: 0;
    font-size: 32px;
    font-weight: normal;
    font-style: normal;
    font-stretch: normal;
    line-height: 0.53;
    letter-spacing: normal;
    color: black;
}

.step4 {
     text-align: center;
     h1{
        width: 482px;    
     }
}

p {
    width: 817px;
    height: 47px;
    font-size: 16px;
    font-weight: normal;
    font-style: normal;
    font-stretch: normal;
    line-height: 1.25;
    letter-spacing: normal;
    color: #554d56;
    margin: 8px 0;
    b {
        width: 817px;
        height: 39.8px;
        font-size: 16px;
        font-style: normal;
        font-stretch: normal;
        line-height: normal;
        letter-spacing: normal;
        color: $charcoalColor;
    }
}
.error{
    color: #e26363;
    font-size: 16px;
    margin-top: 20px;
    height: auto;
}

.ranking{
    display: flex;
    justify-content: space-between;
    position: relative;
    width: 100%;
    align-items: flex-end;
    margin-top: 13px;
    img {
        width: 40px;
        height: 39px;
        object-fit: contain;
    }
    .lables { 
        display: flex;
        justify-content: space-between;
        position: relative;
        width: 660px;
        .rankLine{
            display: block;
            position: absolute;
            height: 3px;
            bottom: 9px;
            left: 0;
            width: 99%;
            z-index: 1;
            background-color: #ddd;
            .rankProgress{
                background-color: $cerulianColor;
                position: relative;
                height: 3px;
                transition: all .2s ease-out;
            }
        }
    }
}

textarea {
    width: 819px;
    height: 139px;
    border-radius: 3px;
    border: solid 1px #dddddd;
    background-color: #f5f5f5;
    margin-top: 17px;
    padding: 10px;
    box-sizing: border-box;
    font-size: 16px;
}


/* Create a custom radio button */
.container_radio {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: relative;
    cursor: pointer;
    font-size: 20px;
    z-index: 20;
    font-weight: bold;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;   
    color: $charcoalColor;
    input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
    }
    .checkmark_radio {
        margin-top: 10px;
        height: 15px;
        width: 15px;
        border: 3px solid #ddd;
        background-color: white;
        border-radius: 50%;
        transition: all .1s ease-out;
        &:after{
            content: "";
            position: absolute;
            display: none;
        }
    }
    &:hover{
        input ~ .checkmark_radio {
            background-color: white;
            border-color: $cerulianColor;
        }
    }
}


.container_radio input:checked ~ .checkmark_radio,
.checked ~ .checkmark_radio,
.container_radio:hover .checked ~ .checkmark_radio {
    background-color: $cerulianColor;
    border-color: $cerulianColor;
}

.container_radio input:checked ~ .checkmark_radio:after {
    display: block;
}

.container_checkbox {
    display: block;
    position: relative;
    padding-left: 35px;
    margin-bottom: 12px;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    font-size: 16px;
    color: $charcoalColor;
    &:hover{
        input ~ .checkmark_checkbox {
            background-color: #ccc;
        }
    }
    input{
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
    }
    .checkmark_checkbox {
        position: absolute;
        top: 0;
        left: 0;
        height: 20px;
        width: 20px;
        border-radius: 3px;
        background-color: #eee;
        &:after{
            content: "";
            position: absolute;
            display: none;
            left: 8px;
            top: 4px;
            width: 3px;
            height: 8px;
            border: solid white;
            border-width: 0 2px 2px 0;
            -webkit-transform: rotate(45deg);
            -ms-transform: rotate(45deg);
            transform: rotate(45deg);
        }
    }
}

.container_checkbox input:checked ~ .checkmark_checkbox {
    background-color: $cerulianColor;
    &:after{
        display: block;
    }
}

::-webkit-input-placeholder { /* Chrome/Opera/Safari */
    font-size: 16px;
    font-style: italic;
    color: $placeholderColor;
}
::-moz-placeholder { /* Firefox 19+ */
    font-size: 16px;
    font-style: italic;
    color: $placeholderColor;
}
:-ms-input-placeholder { /* IE 10+ */
    font-size: 16px;
    font-style: italic;
    color: $placeholderColor;
}
:-moz-placeholder { /* Firefox 18- */
    font-size: 16px;
    font-style: italic;
    color: $placeholderColor;
}
 
.btnsBlock{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 60px;
    span{
        font-size: 16px;
        font-weight: normal;
        font-style: normal;
        font-stretch: normal;
        line-height: normal;
        letter-spacing: normal;
        color: $cerulianColor;
        font-family: 'PT Sans Narrow', sans-serif;
        cursor: pointer;
    }
    .closeBtn{
        font-size: 20px;
    }
    .sendBtn{
        width: 109px;
        height: 45px;
        cursor: pointer;
    }
}

button {
    border-radius: 3px;
    background-color: $cerulianColor;
    border: none;
    color: white;
    text-align: center;
    text-decoration: none;
    font-family: 'PT Sans Narrow', sans-serif;
    display: inline-block;
    font-size: 20px;
}

</style>