<template>
    <div class="box">
        <TranslateIcon class="box__icon" @click="changeTranslate()" />
        <div class="box__header">
            <h2>{{ translate.appTitle }}</h2>
        </div>

        <input type="text" :placeholder="translate.inputPlaceholder" class="input" @keyup.enter="newNumber()">
        <button @click="newNumber()" class="btn">{{ translate.addButton }}</button>
        <p class="errorbox">{{ errorMessage }}</p>

        <div class="box__weight" v-if="weightStatus === true">
        <input type="text" :placeholder="translate.weightPlaceholder" class="input" id="weightinput" @keyup.enter="newNumber()">
        <p class="errorbox">{{ weightErrorMessage }}</p>
        </div>

        <div class="box__checkweight">
            <input type="checkbox" class="switch" id="weight" @click="checkWeight()">
            <label for="weight">{{ translate.weightCheckbox }}</label>
        </div>

        <p class="box__emptyerror">{{ listStatus }}</p>

        <div class="box__container" v-if="numbers.length > 0">
            <div v-for="number in numbers" :key="number.id" class="box__container__item">
                <p class="input p">{{ number.number }} <span style="font-size: 12px; color: gray; margin-left: 2px;">({{ translate.weight }}: {{ number.weight }})</span></p>
                <button @click="removeNumber(number.id)" class="btn">{{ translate.removeButton }}</button>
            </div>
        </div>

        <p class="box__result" v-if="numbers.length > 0">
            <span style="font-size: 15px">{{ translate.numbersCount }}: {{ numbers.length }}</span><br>
            <span style="font-size: 17px">{{ translate.median }}: {{ median }}</span><br>
            {{ translate.average }}: {{ average }}
        </p>
    </div>
</template>

<script>
import EnglishTranslate from '../locales/en.json'
import PolishTranslate from '../locales/pl.json'

import GlobalConfig from '../config/global.json'

import TranslateIcon from  '@/components/TranslateIcon.vue'
    export default {
        name: 'Main',
        data: function() {
            return {
                translate: EnglishTranslate,
                config: GlobalConfig,
                errorMessage: '',
                weightErrorMessage: '',
                listStatus: '',
                weightStatus: false,

                numbers: [],
                average: 0,
                length: 0,
            }
        },
        components: {
            TranslateIcon
        },
        methods: {
            checkTranslate() {
                if (navigator.language == "pl-PL") {
                    this.translate = PolishTranslate;
                } else {
                    this.translate = EnglishTranslate;
                }
            },
            changeTranslate() {
                if (this.translate.lang == "en") {
                    this.translate = PolishTranslate;
                    this.listStatus = this.translate.listStatusEmpty;
                } else { 
                    this.translate = EnglishTranslate;
                    this.listStatus = this.translate.listStatusEmpty;
                }
            },
            checkWeight() {
                const checkbox = document.querySelector('#weight');

                if (checkbox.checked) {
                    this.weightStatus = true;
                } else {
                    this.weightStatus = false;
                }
            },
            newNumber: function() {
                const input = document.querySelector('input');
                const id = Math.random();
                let val = input.value;
                let weight = 1;


                if (this.weightStatus === true) {
                    const weightInput = document.getElementById('weightinput');

                    if (!weightInput.value) {
                        this.weightErrorMessage = this.translate.errorField;
                        return 0;
                    } else {
                        weight = weightInput.value;
                    }

                    if (isNaN(weight)) {
                        this.weightErrorMessage = this.translate.errorNaN;
                    }
                }

                if (val.search(",") >= 0) {
                    val = val.replace(/,/g, '.');
                }

                if (!val) {
                    this.errorMessage = this.translate.errorField;
                    return 0;
                }
                if (isNaN(val)) {
                    this.errorMessage = this.translate.errorNaN;
                    return 0;
                }
                
                const number = parseFloat(val).toFixed(2);
                const decorateNumber = number.replace(/\./g, ',');

                const numberObject = {
                    value: parseFloat(number),
                    number: decorateNumber,
                    weight: parseInt(weight),
                    id: id,
                }

                this.numbers.push(numberObject);
                this.listStatus = this.translate.listStatusNumbers;
                this.length = parseInt(this.length + parseInt(weight));

                if (this.config.autoClearNumber === true) {
                    input.value = '';
                }

                if (this.config.autoClearWeight === true && this.weightStatus === true) {
                    weightInput.value = '';
                }

                this.weightErrorMessage = "";
                this.errorMessage = "";

                this.getAverage();
            },
            removeNumber: function(newID) {
                for (let i = 0; i < this.numbers.length; i++) {
                    if (this.numbers[i].id === newID) {
                        this.length = this.length - this.numbers[i].weight;
                        this.numbers.splice(i, 1);
                    }
                }

                if (this.numbers == 0) {
                    this.listStatus = this.translate.listStatusEmpty;
                }
                this.getAverage();
            },
            getAverage: function() {
                let i = 0;
                let average;
                let length = this.length;
                let arr = [];
                for (let suma = 0; i < this.numbers.length; i++) {
                    let val = parseInt(this.numbers[i].value) * parseInt(this.numbers[i].weight)
                    suma = suma + val;
                    arr.push(this.numbers[i].value);
                    const average = suma / length;
                    this.average = average.toFixed(2).replace(/\./g, ',');;
                }

                const arrSort = arr.sort();
                const len = arrSort.length;
                const mid = Math.ceil(len / 2);
                let median = len % 2 == 0 ? (arrSort[mid] + arrSort[mid - 1]) / 2 : arrSort[mid - 1];
                median = parseFloat(median).toFixed(2).toString().replace(/\./g, ',');

                this.median = median;
            },
        },
        mounted() {
            this.checkTranslate();
            this.listStatus = this.translate.listStatusEmpty;
        }
    }
</script>

<style lang="scss" scoped>
.box {
    padding: 30px;
    overflow: auto;
    background-color: #fff;
    width: 450px;
    height: 82vh;
    border-radius: 5px;
    position: relative;

    &__result {
        padding: 15px 0;
        border-top: 1px solid #000;
        text-align: right;
        margin-top: 20px;
        z-index: 99;
        font-size: 19px;
        font-weight: 500;
    }

    &__checkweight {
        margin-top: 15px;
        font-family: 'Inter', 'Poppins';

        label {
            margin-left: 10px;
        }
    }

    &__icon {
        width: 35px; 
        height: 35px;
        fill: #1a1a1a;
        position: absolute;
        right: 30px;
        cursor: pointer;
    }

    img {
        width: 50px;
        display: inline-block;
        transform: translateY(13px);
    }

    h2 {
        font-weight: 500;
        text-align: center;
        margin-top: 60px;
        margin-bottom: 50px;
    }

    &__emptyerror {
        margin-top: 30px;
        opacity: 0.8;
        text-align: center;
    }

    .p {
        display: inline-block;
    }

    .none {
        display: none;
    }

    &__container {
        margin-top: 10px;
    }
}

.errorbox {
    color: red;
    font-size: 14px;
    margin-top: 5px;
}

@media (max-width: 460px) {
    .box {
        width: 100%;
        height: 100vh;
        border-radius: 0;
        
    }
}
@media (max-width: 350px) {
    .box {
        padding: 15px;

    }
}
</style>