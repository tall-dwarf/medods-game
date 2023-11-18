<template>
    <div @click="onClick" :class="`simon-item ${color} ` + getActiveClass">
        <audio ref="soundRef" :src="'/sounds/' + sound"></audio>
    </div>
</template>

<script>

export default {
    props: ['color', 'isActive', 'sound',],
    emits: ['click', 'stop'],

    data() {
        return {
            soundRef: null,
            isVisible: false,
        }
    },

    methods: {
        onClick() {
            this.$emit('click')
        }
    },

    computed: {
        getActiveClass() {
            return this.isActive ? 'simon-item--active' : ''
        }
    },

    watch: {
        isActive(currentValue) {
            if (currentValue) {
                this.$refs.soundRef.currentTime = 0
                this.$refs.soundRef.play();
                setTimeout(() => {
                    this.$emit("deactivate")
                }, 200);
            }
        }
    }
}

</script>

<style>
.simon-item {
    flex: 1 1 50%;
    cursor: pointer;
    opacity: 0.4;
}

.simon-item--active {
    opacity: 1;
}
</style>