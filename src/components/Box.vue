<template>
    <div class="box">
        <div class="box__header">
            <h2>{{ appTitle }}</h2>
        </div>

        <input type="text" placeholder="New number" class="input">
        <button @click="newNumber()" class="btn">Add</button>
        <p class="errorbox">{{ errorMessage }}</p>

        <p class="box__emptyerror">{{ listStatus }}</p>

        <div class="box__container">
            <div v-for="number in numbers" :key="number.id" class="box__container__item">
                <p class="input p">{{ number.v }}</p>
                <button @click="removeNumber(number.id)" class="btn">Remove</button>
            </div>
        </div>

        <p class="box__result none">{{ average }}</p>
    </div>
</template>

<script>
    export default {
        name: 'Box',
        data: function() {
            return {
                appTitle: "Average Calculator",
                listStatus: "List is empty",
                version: "v1.0.2",
                errorMessage: '',

                numbers: [],
                average: 0,
            }
        },
        methods: {
            newNumber: function() {
                const input = document.querySelector('input');
                const val = input.value;
                console.log(val);
                const id = Math.random();
                if (val.search(",") >= 1) {
                    this.errorMessage = "Use a period as a comma!";
                } else if (isNaN(val)) {
                        this.errorMessage = "Value is not a number!";
                } else if (!val) {
                    this.errorMessage = "Field is empty!";
                } 
                else {
                    this.numbers.push({v: parseFloat(val), id: id});
                    this.listStatus = "Numbers";
                    this.errorMessage = "";
                    this.getAverage();
                }
                this.isEmpty();
            },
            removeNumber: function(newID) {
                for (let i = 0; i < this.numbers.length; i++) {
                    if (this.numbers[i].id === newID) {
                        this.numbers.splice(this.numbers[i], 1);
                    }
                }
                this.isEmpty();
                this.getAverage();
            },
            getAverage: function() {
                let i = 0;
                let average;
                for (let suma = 0; i < this.numbers.length; i++) {
                    suma = suma + this.numbers[i].v;
                    const average = suma / this.numbers.length;
                    this.average = average.toFixed(2);
                    console.log(this.average);
                }
                this.isEmpty();
            },
            isEmpty: function() {
                if (this.numbers.length <= 0) {
                    this.listStatus = "List is empty";
                    document.querySelector('.box__result').classList.add('none');
                } else {
                    document.querySelector('.box__result').classList.remove('none');
                }
            },
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