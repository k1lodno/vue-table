<template>
    <div>
        <table>
            <caption>
                <button type="button"
                        class="btn" @click="showModal" id="addBtn">ADD NEW RECORD</button>
            </caption>

            <Popup  v-if="isModalVisible"
                    @close="closeModal" @productData="onData" />

            <Popup  v-if="isEditModalVisible"
                    @close="closeEditModal" @productData="onData" :dataRow="tempRow" :isEdit="isEditModalVisible"/>

            <thead>
            <tr>
                <th>Product Name</th>
                <th>Unit Price</th>
                <th>Units in Stock</th>
                <th>Discontinued</th>
                <th></th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="(row, index) in getRows()" v-bind:key="index">
                <td v-for="col in columns" v-bind:key="col">{{row[col]}}</td>
                <td><button type="button" class="btn" @click="showEditModal(index, row)">EDIT</button>
                    <div class="divider"></div>
                    <button type="button" class="btn" @click="deleteRow(index)">DELETE</button></td>
            </tr>
            </tbody>
        </table>

        <div class="pagination">
            <div class="number"
                 v-for="i in numPages()"
                 v-bind:key="i"
                 v-bind:class="[i === currentPage ? 'active' : '']"
                 v-on:click="changePage(i)">{{i}}</div>
        </div>

    </div>
</template>

<script>
    import Popup from './Modal'
    import Vue from 'vue'

    export default {
        name: "Table",
        components: {
            Popup
        },
        data(){
            return {
                rows: [],
                isModalVisible: false,
                isEditModalVisible: false,
                currentPage: 1,
                elementsPerPage: 5,
                tempIndex: "",
                tempRow: []
            }
        },
        methods: {
            showModal() {
                this.isModalVisible = true;
            },
            showEditModal(index, row) {
                this.isEditModalVisible = true;
                if(row != null){
                    this.tempIndex = index;
                    //this.tempRow[this.tempIndex] = row;
                    this.tempRow.splice(0,1,row);
                }
            },
            closeModal() {
                this.isModalVisible = false;
            },
            closeEditModal() {
                this.isEditModalVisible = false;
            },
            onData(data){
                if(this.isEditModalVisible){
                    Vue.set(this.rows, this.tempIndex, data)
                    //this.rows[this.tempIndex] = data;
                }else {
                    this.rows.push(data);
                }
            },
            deleteRow(index){
                if(confirm("Are you sure to delete this item?")) {
                    if (this.currentPage !== 1) {
                        index = index + (this.currentPage - 1) * this.elementsPerPage;
                        this.rows.splice(index, 1);
                    } else {
                        this.rows.splice(index, 1);
                    }
                }
            },
            numPages() {
                return Math.ceil(this.rows.length / this.elementsPerPage);
            },
            getRows() {
                let start = (this.currentPage-1) * this.elementsPerPage;
                let end = start + this.elementsPerPage;
                return this.rows.slice(start, end);

            },
            changePage(page) {
                this.currentPage = page;
            }
        },
        computed: {
            columns() {
                if (this.rows.length === 0) {
                    return [];
                }
                return Object.keys(this.rows[0])
            }
        }
    }
</script>

<style scoped>
    table {
        font-family: 'Open Sans', sans-serif;
        width: 750px;
        border-collapse: collapse;
        border: 3px solid #44475C;
        margin: 10px 10px 0 10px;
    }

    table th {
        text-transform: uppercase;
        text-align: left;
        background: #44475C;
        color: #FFF;
        padding: 8px;
        min-width: 30px;
    }

    table tbody tr {
        border: 1px solid #44475C;
    }
    table td {
        text-align: left;
        padding: 8px;
        border-right: 2px solid #7D82A8;
    }

    .pagination {
        font-family: 'Open Sans', sans-serif;
        text-align: right;
        width: 750px;
        padding: 8px;
    }

    .number {
        font-weight: bold;
        display: inline-block;
        padding: 4px 10px;
        color: #FFF;
        border-radius: 4px;
        background: #44475C;
        margin: 0px 5px;
        cursor: pointer;
    }
    .number:hover,.number.active {
        background: #717699;
    }

    .btn{
        border: none;
        font-weight: bold;
        font-family: 'Open Sans', sans-serif;
        background: #44475C;
        color: #FFF;
        cursor: pointer;
    }

    .divider{
        width:5px;
        height:auto;
        display:inline-block;
    }

    .btn:hover{
        background: #717699;
    }

    #addBtn{
        float: left;
    }
    caption{
        background: #44475C;
    }
</style>
