<template>
    <div class="simon">
        <div class="simon-main">
            <SimonItem 
            v-for="item, index in items" 
            :sound="item.sound" 
            :color="item.color" 
            :is-active="item.isActive"
            :key="item.id" 
            @click="() => simonItemClick(index)" 
            @deactivate="() => simonItemDeactivate(index)" />
        </div>
        <div class="simon-menu">
            <h3>Уровень {{ lvl }}</h3>
            <select v-model.number="speed">
                <option value="1500">лёгкий</option>
                <option value="1000">средний</option>
                <option value="400">тяжёлый</option>
            </select>
            <button @click="start">старт</button>
        </div>
    </div>
</template>

<script>

import SimonItem from './SimonItem.vue';
import { randomInRange } from "@/lib/random"
import Audio1 from "@/assets/img/sounds/1.mp3"
import Audio2 from "@/assets/img/sounds/2.mp3"
import Audio3 from "@/assets/img/sounds/3.mp3"
import Audio4 from "@/assets/img/sounds/4.mp3"

export default {
    name: 'GameSimon',
    components: { SimonItem },
    data() {
        return {
            items: [
                { id: 1, isActive: false, sound: Audio1, color: 'red', },
                { id: 2, isActive: false, sound: Audio2, color: 'yellow', },
                { id: 3, isActive: false, sound: Audio3, color: 'green', },
                { id: 4, isActive: false, sound: Audio4, color: 'aqua', },
            ],
            speed: 1000,
            lvl: 1,
            sequence: [],
        }
    },

    methods: {

        simonItemDeactivate(itemIndex) {
            this.items[itemIndex].isActive = false
        },

        gameLose() {
            this.lvl = 1
            this.sequence = []
            alert("Вы проиграли")
        },

        lvlUp() {
            this.lvl += 1
            this.start()
        },

        // Обработка клика по одной из кнопок
        simonItemClick(itemIndex) {
            if (this.sequence.length == 0) return null
            if (this.sequence[0] !== itemIndex) return this.gameLose()

            this.items[itemIndex].isActive = true
            this.sequence.shift();
            if (this.sequence.length === 0) {
                this.lvlUp()
            }

        },

        // Генерирует последовательность случайных чисел
        generateSequence() {
            for (let index = 0; index <= this.lvl - 1; index++) {
                this.sequence.push(randomInRange(0, 3))
            }
        },

        // Запуск уровня
        start() {
            this.generateSequence()
            let index = 0;

            const interval = setInterval(() => {
                const activItem = this.items[this.sequence[index]]
                activItem.isActive = true;

                index++

                if (index >= this.sequence.length) {
                    clearInterval(interval);
                }
            }, this.speed);
        },
    }
}

</script>

<style>
.simon {
    display: flex;
    margin: 0 auto;
    align-items: center;
    gap: 20px;
}

.simon-main {
    width: 400px;
    height: 400px;
    border-radius: 50%;
    overflow: hidden;
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
}



</style>