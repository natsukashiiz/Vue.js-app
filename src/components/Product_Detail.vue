<template>
    <div class="card" v-if="product.length == 0">
        <div class="card-body" style="height:55vh">
            <div class="text-center">
                <div class="spinner-grow spinner-grow-sm mx-1"></div>
                <div class="spinner-grow spinner-grow-sm"></div>
                <div class="spinner-grow spinner-grow-sm mx-1"></div>
            </div>
        </div>
    </div>
    <div class="container p-5 bg-white" v-else>
        <div class="container px-4 px-lg-5 my-5  border border-2 p-5">
            <div class="row gx-4 gx-lg-5 align-items-center">
                <div class="col-md-6">
                    <img class="card-img-top mb-5 mb-md-0" :src="`../images/products/${product.img}`" :alt="product.name" />
                </div>
                <div class="col-md-6">
                    <h4 class="fw-bolder">
                        {{product.name}}
                    </h4>
                    <div class="fs-5 mb-2">
                        <h5 class="text-danger">
                            ราคา: ฿{{formatPrice(product.price)}}
                        </h5>
                        <h6 class="text-secondary">
                            คงเหลือ: {{product.qty}} ชิ้น
                        </h6>
                    </div>
                    <div v-if="product.qty > 0 && uid">
                        <form @submit="form">
                            <div class="row mb-3">
                                <label class="col-sm-2 col-form-label">จำนวน</label>
                                <div class="col-sm-10">
                                    <input type="number" class="form-control mx-2" :max="`${product.qty}`" min="1" v-model="form.qty" style="width:70px">
                                </div>
                            </div>
                            <button type="submit" class='btn btn-outline-danger btn-lg me-2 btn-width-100' @click="addCart">หยิบใส่ตะกร้า</button>
                            <button type="submit" class='btn btn-danger btn-lg me-2 btn-width-100' @click="buyCart">ซื้อสินค้า</button>
                        </form>
                        <div :class="`alert alert-${alert.color} mt-2`" v-if="alert">
                            <b>{{alert.msg}}</b>
                        </div>
                    </div>
                    <div v-if="!uid">
                        <router-link to="/Login" class="btn btn-primary">เข้าสู่ระบบ</router-link>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <h4>รายละเอียดสินค้า</h4><hr>
            <table cellpadding="0" cellspacing="0">
                <tbody>
                    <tr>
                        <td>
                            {{product.spec}}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>        
    </div>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            product: [],
            form: {
                id:this.$route.params.id,
                qty:1,
            },
            alert: {
                color:'',
                msg:''
            },
            uid:this.$store.state.user.uid,
        }
    },
    async mounted() {
        let id = this.$route.params.id; 
        await axios
            .get('https://jhgdfjkjkdfasdf.herokuapp.com/product/'+id)
            .then((res) => {
                this.product = res.data.data
            })
    },
    methods: {
        formatPrice(value) {
            return value.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
        },
        
        async addCart(e) {
            e.preventDefault();
            await axios
                .post('https://jhgdfjkjkdfasdf.herokuapp.com/cart',{
                    uid: this.$store.state.user.uid,
                    pid: this.form.id,
                    qty: this.form.qty
                })
                .then((res) => {
                    // console.log(res);
                    if(res.data.data.error !== true){
                        const qty = parseInt(this.$store.state.user.cartQTY)
                        const qty_new = parseInt(this.form.qty)
                        this.$store.commit('setCartQTY', qty+qty_new)
                    
                        this.alert.color = "success"
                        this.alert.msg = "คุณได้ทำการเพิ่มสินค้าลงในตะกร้าแล้ว"
                    }else{
                        this.alert.color = "danger"
                        this.alert.msg = "เพิ่มสินค้าไม่สำเร็จ กรุณาลองใหม่อีกครั้ง!!!"
                    }
                })
        },

        async buyCart(e) {
            e.preventDefault();
            await axios
                .post('https://jhgdfjkjkdfasdf.herokuapp.com/cart',{
                    uid: this.$store.state.user.uid,
                    pid: this.form.id,
                    qty: this.form.qty
                })
                .then((res) => {
                    if(res.data.data.error !== true){
                        this.$router.replace("/Cart")
                    }else{
                        this.alert.color = "danger"
                        this.alert.msg = "เพิ่มสินค้าไม่สำเร็จ กรุณาลองใหม่อีกครั้ง!!!"
                    }
                })
        }
        
    }
}
</script>