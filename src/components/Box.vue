<template>
    <div class="box">
        <TranslateIcon class="box__icon" @click="changeTranslate()" />
        <div class="box__header">
            <h2>{{ translate.appTitle }}</h2>
        </div>

        <input type="text" :placeholder="translate.inputPlaceholder" class="input" @keyup.enter="newNumber()">
        <button @click="newNumber()" class="btn">{{ translate.addButton }}</button>
        <p class="errorbox">{{ errorMessage }}</p>

        <p class="box__emptyerror">{{ listStatus }}</p>

        <div class="box__container" v-if="numbers.length > 0">
            <div v-for="number in numbers" :key="number.id" class="box__container__item">
                <p class="input p">{{ number.dN }}</p>
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

import TranslateIcon from  '@/components/TranslateIcon.vue'
    export default {
        name: 'Box',
        data: function() {
            return {
                translate: EnglishTranslate,
                errorMessage: '',
                listStatus: '',

                numbers: [],
                average: 0,
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
                } else { 
                    this.translate = EnglishTranslate;
                }
            },
            newNumber: function() {
                const input = document.querySelector('input');
                const id = Math.random();
                let val = input.value;

                if (val.search(",") >= 0) {
                    val = val.replace(/,/g, '.');
                }

                if (!val) {
                    this.errorMessage = this.translate.errorField;
                } else if (isNaN(val)) {
                    this.errorMessage = this.translate.errorNaN;
                } else {
                    const number = parseFloat(val).toFixed(2);
                    const decorateNumber = number.replace(/\./g, ',');
                    this.numbers.push({v: parseFloat(number), dN: decorateNumber, id: id});
                    this.listStatus = this.translate.listStatusNumbers;
                    this.errorMessage = "";
                    this.getAverage();
                    input.value = '';
                }
            },
            removeNumber: function(newID) {
                for (let i = 0; i < this.numbers.length; i++) {
                    if (this.numbers[i].id === newID) {
                        this.numbers.splice(i, 1);
                    }
                }
                this.getAverage();
            },
            getAverage: function() {
                let i = 0;
                let average;
                let arr = [];
                for (let suma = 0; i < this.numbers.length; i++) {
                    suma = suma + this.numbers[i].v;
                    arr.push(this.numbers[i].v);
                    const average = suma / this.numbers.length;
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
    height: 85vh;
    border-radius: 5px;

    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    &__result {
        padding: 15px 0;
        border-top: 1px solid #000;
        text-align: right;
        margin-top: 20px;
        z-index: 99;
        font-size: 19px;
        font-weight: 500;
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
        font-weight: 400;
        text-align: center;
        margin-top: 50px;
        margin-bottom: 50px;
        margin-left: 30px;
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