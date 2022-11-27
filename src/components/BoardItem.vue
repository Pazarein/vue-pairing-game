<template>
    <div :class="
        'board-item '
        + (selected ? 'selected ' : '')
        + (closed ? ' closed' : '')
        ">
            <div v-if="iconId" :class="'icon icon-' + iconId"></div>
    </div>
    
</template>

<script>
    export default {
        name: 'BoardItem',
        
        props: {
            iconId: {
                type: Number,
                required: false,
                default: 0,
            },
            selected : {
                type: Boolean,
                required:false,
                default: false,
            },
            closed : {
                type: Boolean,
                required:false,
                default: false,
            },
        }
    }
</script>

<style scoped>
.board-item {
    border: 1px solid #111;
    width: 48px;
    height: 48px;
    position: relative;
}

.selected {
    background-color: rgba(137, 230, 66, 0.849);
}

.closed {
    background-color: rgba(110, 168, 65, 0.436);
}

.icon {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
}

.icon-1 {
    background: url(../assets/cat1.png);
}
.icon-2 {
    background: url(../assets/cat2.png);
}

.icon-3 {
    background: url(../assets/cat3.png);
}


</style>