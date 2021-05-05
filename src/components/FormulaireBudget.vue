<template>
    <!-- <div class="formulaire-budget"> -->
        <!-- <p>{{ this.$props.text }}</p> -->
        <div class="middle mt-8">
            <div class="multi-range-slider">
                <input type="range" id="input-left" min="0" max="100" step="10" value="20">
                <input type="range" id="input-right" min="0" max="100" step="10" value="75">

                <div class="slider">
                    <div class="track"></div>
                    <div class="range"></div>
                    <div class="thumb left"></div>
                    <div class="thumb right"></div>

                    <div class="value left">25K€</div>
                    <div class="value right">75K€</div>
                </div>
            </div>
        </div>
    <!-- </div> -->
</template>

<script>
    export default {
        name: 'FormulaireBudget',
        mounted() {
            // this.setLeftValue();
            // this.setRightValue();
            this.YourBudgetFunction();
        },
        methods: {
            YourBudgetFunction() {
                var inputLeft = document.querySelector("#input-left");
                var inputRight = document.querySelector("#input-right");

                var thumbLeft = document.querySelector('.slider .thumb.left');
                var thumbRight = document.querySelector('.slider .thumb.right');

                var valueLeft = document.querySelector(' .slider .value.left');
                var valueRight = document.querySelector(' .slider .value.right');

                var range = document.querySelector('.slider .range');

                function setLeftValue() {
                    var _this = inputLeft;
                    var min = parseInt(_this.min);
                    var max = parseInt(_this.max);

                    _this.value = Math.min(parseInt(_this.value), parseInt(inputRight.value))

                    var percent = ((_this.value - min) / (max - min)) * 100;

                    thumbLeft.style.left = percent + "%"
                    range.style.left = percent + "%"
                    valueLeft.style.left = percent + "%"

                    valueLeft.innerHTML = `${Number(percent * 3)}K€`;
                }
                setLeftValue();

                function setRightValue() {
                    var _this = inputRight;
                    var min = parseInt(_this.min);
                    var max = parseInt(_this.max);

                    _this.value = Math.max(parseInt(_this.value), parseInt(inputLeft.value) - 1)

                    var percent = ((_this.value - min) / (max - min)) * 100;

                    thumbRight.style.right = (100 - percent) + "%"
                    range.style.right = (100 - percent) + "%"
                    valueRight.style.right = (100 - percent) + "%"

                    valueRight.innerHTML = `${Number(percent * 3)}K€`;

                }
                setRightValue();

                inputLeft.addEventListener("input", setLeftValue);
                inputRight.addEventListener("input", setRightValue);

                inputLeft.addEventListener('mouseover', function () {
                    thumbLeft.classList.add('hover');
                });
                inputLeft.addEventListener('mouseout', function () {
                    thumbLeft.classList.remove('hover');
                });
                inputLeft.addEventListener('mousedown', function () {
                    thumbLeft.classList.add('active');
                });
                inputLeft.addEventListener('mouseup', function () {
                    thumbLeft.classList.remove('active');
                });

                inputRight.addEventListener('mouseover', function () {
                    thumbRight.classList.add('hover');
                });
                inputRight.addEventListener('mouseout', function () {
                    thumbRight.classList.remove('hover');
                });
                inputRight.addEventListener('mousedown', function () {
                    thumbLeft.classList.add('active');
                });
                inputRight.addEventListener('mouseup', function () {
                    thumbRight.classList.remove('active');
                });

            },
        },
        props: {
            text: {
                default: 'default text'
            }
        }
    }
</script>

<style lang="css" scoped>

.middle {
    position: relative;
    width: 80%;
    max-width: 500px;
    height: auto;
}

.middle .slider {
    position: relative;
    z-index: 1;
    height: 10px;
    margin: 0 15px;
}

.middle .slider .track {
    position: absolute;
    z-index: 1;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    border-radius: 5px;
    background-color: #d4d4d4;
}

.middle .slider .range {
    position: absolute;
    z-index: 2;
    left: 25%;
    right: 25%;
    top: 50%;
    transform: translateY(-50%);
    border-radius: 5px;
    height: 4px;
    background-color: #030303;
}

.middle .slider .thumb {
    position: absolute;
    z-index: 3;
    width: 15px;
    height: 15px;
    background-color: #030303;
    border-radius: 50%;
    box-shadow: 0 0 0 0 rgba(98, 0, 238, .1);
    transition: box-shadow 0.3s ease-in-out;
    transform: translate(0px, -2px);
}

.middle .slider .thumb.left {
    left: 25%;
}

.middle .slider .thumb.right {
    right: 25%;
}

.middle .slider .value {
    position: absolute;
    top: -20px;
    font-size: 10px;
}

.middle .slider .value.left {
    left: 25%;
    transform: translateX(-25%);
}

.middle .slider .value.right {
    right: 25%;
    transform: translateX(25%);
}

.slider .thumb.hover {
    box-shadow: 0 0 0 40px rgba(150, 146, 156, 0.2);
}

input[type=range] {
    position: absolute;
    -webkit-appearance: none;
    z-index: 2;
    height: 10px;
    width: 100%;
    pointer-events: none;
    opacity: 0;
}

input[type=range]::-webkit-slider-thumb {
    width: 30px;
    height: 30px;
    border-radius: 0;
    border: 0 none;
    background-color: red;
    -webkit-appearance: none;
    pointer-events: all;
}

</style>
