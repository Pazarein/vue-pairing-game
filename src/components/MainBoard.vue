<template>
    <div class="lvl"> Уровень: {{ lvl }} </div>
    <div class="win" v-if="gameStatus === 1">Вы выиграли! Поздравляем!</div>
    <div class="board" :style="{ width: 51 * size + 'px', height:51 * size + 'px'}">
        
        <BoardItem 
            v-for="(cell,index) in cells" 
            :key="cell + '-' + index" 
            :iconId="cell"
            @mousedown="mousedown(index)"
            @mouseup="mouseup(index)"
            @mousemove="go(index)"
            :selected="checkRoad(index)"
            :closed="isRoadClosed(index)"
        />

        
        
        <div class="reload" @click="reload()">Сбросить уровень</div>
    </div>
</template>

<script>
    import BoardItem from './BoardItem.vue';
    import {ref} from 'vue';

    

    export default {
        name: 'MainBoard',

        components: {
            BoardItem,
        },

        setup() {
            let cells = ref([3,1,0,1,0,0,0,0,2,0,0,0,2,0,0,3]);
            const path = ref([]);
            const size = ref(4);
            const closedPath = ref([]);
            let lvl = ref(1);
            const maxLvl = 2;
            let gameStatus = ref(0);

            const mousedown = (index) => {
                path.value = [];
                if(cells.value[index] && !isRoadClosed(index)){
                    path.value.push(index);
                }

            }

            const mouseup = (index) => {
                console.log(index, path.value[0], cells.value[index], cells.value[path.value[0]]);
                if (index !== path.value[0] && cells.value[index] === cells.value[path.value[0]]){
                    closedPath.value = closedPath.value.concat(path.value);
                }


                path.value = [];
                checkLvl();
            }

            const go = (index) => {

                if (path.value.length) {
                    const lastIndex = path.value[path.value.length - 1];

                    if ((Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value) && !isRoadClosed(index)) {
                        path.value.push(index);
                    }

                    if (isRoadClosed(index)
                     || (cells.value[index] && cells.value[index] !== cells.value[path.value[0]])){
                        path.value = [];
                    }
                }

            }

            const checkRoad = (index) => {
                return path.value.findIndex(p => p === index) > -1;
            }

            const isRoadClosed = (index) => {
                return closedPath.value.findIndex(p => p === index) > -1;
            }

            const checkLvl = () => {
                let completed = true;

                cells.value. forEach((cell,index) => {
                    if (cell && !isRoadClosed(index)){
                        completed = false;
                    }
                })
                if (completed) {
                    goToNextLvl();
                }
            }

            const goToNextLvl = () => {
                lvl.value += 1;
                gameStatus.value = 0;

                if (lvl.value > maxLvl){
                    lvl.value = 1;
                    gameStatus.value = 1;
                }

                start(lvl.value);
            }

            const start = (lvl) => {
                if (lvl === 1){
                    cells.value = [3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3];
                    size.value = 4;
                }
                if (lvl === 2){
                    cells.value = [3, 1, 1, 0, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3, 0, 0, 0, 0, 0, 3, 0, 3, 0];
                    size.value = 5;
                }

                
                path.value = [];
                closedPath.value = [];
            }

            start(lvl.value);

            const reload = () => {
                start(lvl.value);
            }

            return {
                cells,
                mousedown,
                mouseup,
                go,
                path,
                checkRoad,
                isRoadClosed,
                lvl,
                reload,
                start,
                size,
                gameStatus,
            }
        }
    }
</script>

<style  scoped>
.board {
    display: flex;
    flex-wrap: wrap;
    width: 202px;
    height: 202px;
    margin: 20px auto;

}

.lvl {
    margin: 30px auto;
    font-size: 20px;
}

.reload{
    margin: 30px auto;
    text-decoration: underline;
    cursor: pointer;
    font-size: 20px;
}

.win{
    font-weight: 700;
}
</style>