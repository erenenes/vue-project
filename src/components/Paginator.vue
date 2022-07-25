<script >
export default {
    props: {
        currentPage: {
            type: Number,
            default:1
        },
        totalPage: {
            type: Number,
            default:20
        },
        edges: {
            type: Number,
            default:1
        },
        closeToCurrentPage: {
            type: Number,
            default:1
        }
    },
    data() {
        return {
            currentPageObj: this.currentPage,
            totalPageObj: this.totalPage,
            edgesObj: this.edges,
            closeToCurrentPageObj: this.closeToCurrentPage,
            paginatorData: [],
            paginatorDataWithDots: [],
        }
    },

    created() {
        this.buildPaginator();
    },
    methods: {
        buildPaginator: function () {
            this.paginatorData = [];
            this.paginatorDataWithDots = [];
            var exIndex;
            var left = this.currentPageObj - this.closeToCurrentPageObj;
            var right = this.currentPageObj + this.closeToCurrentPageObj + 1;
            var startEdgePoint = 1 + this.edgesObj;
            var finishEdgePoint = this.totalPageObj - this.edgesObj;


            for (var i = 1; i <= this.totalPageObj; i++) {

                if (i < startEdgePoint || i > finishEdgePoint) {
                    this.paginatorData.push({ value: i, disable: 1, text: i.toString() })
                }
                if (i >= left && i < right) {
                    if (!this.paginatorData.some(data => data.value == i)) {
                        this.paginatorData.push({ value: i, disable: 1, text: i.toString() })
                    }
                }
            }

            for (let i of this.paginatorData) {
                if (exIndex) {
                    if (i.value - exIndex.value !== 1) {
                        console.log(i.value, "----", exIndex.value)
                        this.paginatorDataWithDots.push({ value: 0, disable: 0, text: '...' });
                    }
                }
                this.paginatorDataWithDots.push(i)
                exIndex = i;
            }
            console.log("Created Paginator", this.paginatorDataWithDots);
        },
        handleCurrentPage: function (operation) {
            if (operation == "-" && this.currentPageObj > 1)
                this.currentPageObj -= 1;
            if (operation == "+" && this.currentPageObj < this.totalPageObj)
                this.currentPageObj += 1;
            this.buildPaginator();
        },
        handleTotalPage: function (operation) {
            if (operation == "-" && this.totalPageObj > 1)
                this.totalPageObj -= 1;
                if(this.currentPageObj > this.totalPageObj)
                    this.currentPageObj=this.totalPageObj
            if (operation == "+")
                this.totalPageObj += 1;
            this.buildPaginator();
        },
        handleEdges: function (operation) {
            if (operation == "-")
                this.edgesObj -= 1;
            if (operation == "+")
                this.edgesObj += 1;
            this.buildPaginator();
        },
        handleClosetoCurrentPage: function (operation) {
            if (operation == "-")
                this.closeToCurrentPageObj -= 1;
            if (operation == "+")
                this.closeToCurrentPageObj += 1;
            this.buildPaginator();
        },
        paginatorClick:function(pageNumber){
            this.currentPageObj =pageNumber;
            this.buildPaginator();
        }
    }

}
</script>

    
    <template>
    <div id="paginatorMain">
        <div>
            <h2 class="green">PAGINATOR</h2>
        </div>
        <div class="flex grid">
            <button v-on:click="handleCurrentPage('-')"> - </button>
            <h3> Current Page:{{ currentPageObj }} </h3>
            <button v-on:click="handleCurrentPage('+')"> + </button>
        </div>
        <div class="flex grid">
            <button v-on:click="handleTotalPage('-')"> - </button>
            <h3> Total Page:{{ totalPageObj }} </h3>
            <button v-on:click="handleTotalPage('+')"> + </button>
        </div>
        <div class="flex grid">
            <button v-on:click="handleEdges('-')"> - </button>
            <h3> Edges:{{ edgesObj }} </h3>
            <button v-on:click="handleEdges('+')"> + </button>
        </div>
        <div class="flex grid">
            <button  v-on:click="handleClosetoCurrentPage('-')"> - </button>
            <h3> Close to Current Page:{{ closeToCurrentPageObj }} </h3>
            <button v-on:click="handleClosetoCurrentPage('+')"> + </button>
        </div>

    </div>
    <div id="paginator">
            <div>
                <button @click="handleCurrentPage('-')">&#8592;</button>
            </div>
            <div v-for="item in this.paginatorDataWithDots" :key="item.value">
                <button :class="item.value == this.currentPageObj ? 'active':''" v-if="item.disable" @click="paginatorClick(item.value)">{{ item.text }}</button>
                <button v-if="!item.disable" class="disableButton" >{{ item.text }}</button>

                
            </div>
                <button @click="handleCurrentPage('+')">&#8594;</button>

        </div>
</template>


<style scoped>
h1 {
    font-weight: 500;
    font-size: 2.6rem;
    top: -10px;
}

h3 {
    font-size: 1.2rem;
}

.grid {
    display: grid;
    grid-template-columns: auto auto auto;
    padding: 10px;
}

.grid h3 {
    width: 75%;
}

button {
    padding: 5px 15px;
    font-size: 20px;
    color: #34495e;
    border-radius: 204px;
    font-weight: 900;
    border: 1px solid #34495e;
    background-color: #41b883;
    margin: 5px;
}

.flex {
    display: flex;
    align-items: center;
}

#paginator {
    display: flex;
}

#paginator button{
    padding: 5px 15px;
    font-size: 20px;
    border-radius: 10px;
    font-weight: 600;
    color: #41b883;
    border: 1px solid #fff;
    background-color: #fff;
    display: flex;
    align-items: center;
}

#paginator button:hover{
    border: 1px solid #41b883;
    background-color: rgba(255, 255, 255, 0.634);
}

#paginator .active{
    background-color: #e4e4e4b8 !important;
}

.disableButton:hover{
    border: 1px solid #fff !important;
    background-color: #fff !important;
}



</style>