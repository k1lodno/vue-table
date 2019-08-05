<template>
    <transition name="modal-fade">
        <div class="modal-backdrop">
            <div class="modal"
                 role="dialog"
                 aria-labelledby="modalTitle"
                 aria-describedby="modalDescription">

                <header
                        class="modal-header"
                        id="modalTitle">
                    <slot name="header">
                        Edit
                        <button
                                type="button"
                                class="btn-close"
                                @click="close"
                                aria-label="Close modal">x</button>
                    </slot>
                </header>

                <section
                        class="modal-body"
                        id="modalDescription">
                    <slot name="body">
                        <label>Product Name
                            <input type="text" id="productName" v-model="Name"></label><br>
                        <label>Unit Price
                            <span class="inputPrice">$<input type="number" step="1" min="1" id="unitPrice" v-model="Price"></span></label>
                        <br>
                        <label>Units In Stock
                            <input type="number"  step="1" min="0" id="unitsInStock" v-model="Stock"></label>
                        <br>
                        <label>Discontinued
                            <input type="checkbox" id="isDiscontinued" v-model="Disc"></label>
                    </slot>
                </section>

                <footer class="modal-footer">
                    <slot name="footer">
                        <button
                                type="button"
                                class="bottom-btn"
                                @click="close"
                                aria-label="Close modal">
                            CANCEL
                        </button>
                        <button
                                type="button"
                                class="bottom-btn"
                                @click="update"
                                aria-label="Update modal">
                            UPDATE
                        </button>
                    </slot>
                </footer>

            </div>
        </div>
    </transition>
</template>

<script>

    export default {
        name: "Popup",
        data(){
            return {
                tempName: "",
                tempPrice: "",
                tempStock: 0,
                tempDisc: false,
            }
        },
        props: {
            dataRow: Array,
            isEdit: Boolean
        },
        methods: {
            close() {
                this.$emit('close');
            },
            update(){
                if((this.tempName && this.tempPrice) !== "" ) {
                    this.$emit("productData", {
                        productName: this.tempName,
                        unitPrice: "$" + this.tempPrice,
                        unitsInStock: this.tempStock,
                        discontinued: this.tempDisc
                    });
                    this.$emit('close');
                }else{
                    alert("Fill out all the fields");
                }
            },
        },
        computed:{
            Name: {
                get: function () {
                    if(this.isEdit){
                        return this.tempName = this.dataRow[0].productName;
                    }
                    return this.tempName;
                },
                set: function (val) {
                    return this.tempName = val;
                }
            },
            Price:{
                get: function () {
                    if(this.isEdit){
                        return this.tempPrice = this.dataRow[0].unitPrice.substr(1);
                    }else {
                        return this.tempPrice;
                    }
                },
                set: function (val) {
                    return this.tempPrice = val;
                }
            },
            Stock:{
                get: function () {
                    if(this.isEdit){
                        return this.tempStock = this.dataRow[0].unitsInStock;
                    }else {
                        return this.tempStock;
                    }
                },
                set: function (val) {
                    return this.tempStock = val;
                }
            },
            Disc:{
                get: function () {
                    if(this.isEdit){
                        return this.tempDisc = this.dataRow[0].discontinued;
                    }else {
                        return this.tempDisc;
                    }
                },
                set: function (val) {
                    return this.tempDisc = val;
                }
            }
        }
    }

</script>

<style scoped>

    .modal-backdrop {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background-color: rgba(0, 0, 0, 0.3);
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .modal {
        background: #FFFFFF;
        box-shadow: 2px 2px 20px 1px;
        overflow-x: auto;
        display: flex;
        flex-direction: column;
    }

    .modal-header{
        font-family: 'Open Sans', sans-serif;
        padding: 15px;
        display: flex;
        border-bottom: 1px solid #7D82A8;
        color: #44475C;
        justify-content: space-between;
    }

    .modal-footer {
        padding: 15px;
        display: flex;
        border-top: 1px solid #7D82A8;
        justify-content: flex-end;
    }

    .modal-body {
        position: relative;
        padding: 20px 10px;
        text-align:right;
    }

    .btn-close {
        border: none;
        font-size: 20px;
        cursor: pointer;
        font-weight: bold;
        color: #44475C;
        background: transparent;
    }

    .bottom-btn {
        color: white;
        background: #44475C;
        border: 1px solid #7D82A8;
        border-radius: 2px;
    }

    #unitPrice, #unitPrice:hover, #unitPrice:focus, #unitPrice:active {
        border: 0;
        outline: none;
        outline-offset: 0;
        color: #555;
        font-size: 13px;
    }

    .inputPrice {
        border: 1px solid gray;
        color: #999;
        font-size: 17px;
    }

    input{
        margin-bottom:5px;
    }
    .modal-fade-enter-active {
        transition: opacity 0.5s;
    }
    .modal-fade-leave-active {
        transition: opacity 0.5s;
    }
    .modal-fade-enter, .modal-fade-leave-to {
        opacity: 0;
    }
</style>

