<template>
    <div class="box">
        <div class="box__header">
            <h2>{{ appTitle }}</h2>
        </div>

        <input type="number" placeholder="New number" class="input">
        <button @click="newNumber(), getAverage(), isEmpty()" class="btn">{{ addButtonText }}</button>

        <p class="box__emptyerror">{{ listIsEmpty }}</p>

        <div class="box__container">
            <div v-for="number in numbers" :key="number.id" class="box__container__item">
                <p class="input p">{{ number.v }}</p>
                <button @click="removeNumber(number.id), getAverage(), isEmpty()" class="btn">{{ removeButtonText }}</button>
            </div>
        </div>

        <div class="box__result none">
            <p class="box__result__p">{{ average }}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Box',
        data: function() {
            return {
                appTitle: "Average Calculator",
                addButtonText: "Add",
                removeButtonText: "Remove",
                numberCountText: "Numbers",
                listIsEmpty: "List is empty",
                version: "v1.0.0",

                numbers: [],
                average: 0,
            }
        },
        methods: {
            newNumber: function() {
                const input = document.querySelector('input');
                const val = input.value;

                const id = Math.random();
                this.numbers.push({v: parseInt(val, 10), id: id});

                this.listIsEmpty = "Numbers";
            },
            removeNumber: function(ide) {
                for (let i = 0; i < this.numbers.length; i++) {
                    if (this.numbers[i].id === ide) {
                        this.numbers.splice(this.numbers[i], 1);
                    }
                }
            },
            getAverage: function() {
                let i = 0.00;
                let average;
                for (let suma = 0; i < this.numbers.length; i++) {
                    suma = suma + this.numbers[i].v;
                    const average = suma / this.numbers.length;
                    this.average = average.toFixed(2);
                    // console.log(average);
                }    
            },
            isEmpty: function() {
                if (this.numbers.length <= 0) {
                    this.listIsEmpty = "List is empty";
                    document.querySelector('.box__result').classList.add('none');
                } else {
                    document.querySelector('.box__result').classList.remove('none');
                }
            }
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
        font-size: 18px;
        font-weight: 500;
    }

    img {
        width: 50px;
        display: inline-block;
        transform: translateY(13px);
    }

    h2 {
        font-weight: 400;
        text-align: center;
        // display: inline-block;
        margin-top: 30px;
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