<template>
    <ScrollView>
        <StackLayout class="container">
            <!--        for each order display -->
            <StackLayout row="2" flexDirection="column" orientation="vertical" marginTop="40">
                <Label text="Your order" class="title" fontSize="20" fontWeight="bold" margin="5 5 10 18"/>

                <ScrollView>
                    <StackLayout class="card-body" justifyContent="space-around" alignItems="center"
                        v-if="orderList && orderList.length > 0">
                        <CartBox v-for="order in orderList" :key="order.id" :order="order" />
                    </StackLayout>
                    <Label v-else class="info" text="No products available" />
                </ScrollView>
            </StackLayout>
        </StackLayout>
    </ScrollView>
</template>

<script>
import { apiUrl } from '../../../config/config';
import { getString } from "@nativescript/core/application-settings";
import CartBox from '~/container/CartBox';

export default {
    data() {
        return {
            orderList: [],
        }
    },
    components: {
        CartBox
    },
    methods: {
        async listOrders(token) {
            try {
                const response = await fetch(`${apiUrl}order/?token=${token}`, {
                    method: "GET",
                });
                if (response.ok) {
                    this.orders = await response.json();
                    this.orders.forEach((order) => {
                        this.orderList.push({
                            id: order.id,
                            status:order.status,
                            totalCost: order.totalPrice,
                            // get short date
                            orderdate: order.createdDate.substring(0, 10),
                            // get image of the first orderItem of the order
                            imageURL: order.orderItems[0] ? order.orderItems[0].product.imageURL : "",
                            // get total items
                            totalItems: order.orderItems.length
                        })
                    })
                } else {
                    alert("Error in fetching data");
                }
            } catch (error) {
                alert(error);
            }
        }
    },
    mounted() {
        try {
            const token = getString('token');
            if (!token || token == "") {
                // alert("Please login");
            }
            else {
                this.listOrders(token);
            }
        } catch (error) {
            alert(error)
        }

    },
}

</script>