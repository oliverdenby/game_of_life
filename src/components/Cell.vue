<template>
    <div 
        ref="cell" 
        @click="handleCellClicked($event)" 
        class="cell" 
        :class="{ alive: isAlive, dead: !isAlive }">
    </div>
</template>

<script>
export default {
    props: ['isAlive_'],
    data() {
        return {
            isAlive: this.isAlive_
        }
    },
    methods: {
        handleCellClicked(event) {
            this.$emit('cellClicked', event.target.id)
            this.isAlive = !this.isAlive
        }
    },
    watch: {
        isAlive_: function (newVal) {
            this.isAlive = newVal;
        }
    }        
}
</script>