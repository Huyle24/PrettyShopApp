<template>
    <Page>
        <ActionBar class="action-bar">
            <NavigationButton visibility="hidden" />
            <GridLayout columns="auto, *, auto">
                <Image src="https://prettyshopfemobilev2.vercel.app/icon/house.png" class="fas left-aligned"
                    @tap="onBackHome" col="0" />
                <Label text="Shipment Details" col="1" />
                <!-- <Image src="https://prettyshopfemobilev2.vercel.app/icon/house.png" class="fas"   @tap="onBackHome" col="2"   /> -->
            </GridLayout>
        </ActionBar>
        <GridLayout class="page__content">
            <StackLayout class="form-container" row="0">
                <Label class="label" text="fullname:" />
                <TextField class="text-field" v-model="fullname" autocapitalizationType="none" isEnabled="false"/>

                <Label class="label" text="addpress:" />
                <TextField class="text-field" v-model="addpress" autocapitalizationType="none" isEnabled="false"/>

                <Label class="label" text="phone:" />
                <TextField class="text-field mb-20" v-model="phone" autocapitalizationType="none" isEnabled="false"/>

                <GridLayout columns="*, *" height="200">
                    <StackLayout col="1" :class="'borderr ' + (selectedPage === 'payment' ? ' active' : '')" width="100%"
                        @tap="onChangeActive('payment')">
                        <Label class="label" text="Paypal" />
                        <Label class="label-span" text="Payment via paypal, visa" />
                        <Image src="https://prettyshopfemobilev2.vercel.app/img/cashless-payment.png" class="img" />
                    </StackLayout>
                </GridLayout>
                <StackLayout>
                    <Button class="submit-button" text="Submit" @tap="submitPayment" />
                </StackLayout>
            </StackLayout>
        </GridLayout>

    </Page>
</template>

<script>
import { setString, getString } from "@nativescript/core/application-settings";
import { apiUrl } from "~/config/config";
import WebPayMent from './WebPayMent';
import Home from "~/components/Home";

export default {
    props: ["sessionId", "id", "fullname", "addpress", "phone"],
    data() {
        return {
            url: "https://prettyshopfemobilev2.vercel.app/checkoutmobile/" + getString('token'),
            checkoutBodyArray: [], fullname: "", addpress: "", phone: "", selectedPage: 'payment'
        }
    },
    methods: {
        onChangeActive(component) {
            this.selectedPage = component;
        },
        onBackHome() {
            this.$navigateTo(Home, {
                clearHistory: true
            })
        },
        async getSection() {
            // alert(this.sessionId);
            setString('sessionId', this.sessionId);
        },
        async submitPayment() {
            this.$navigateTo(WebPayMent, {
                props: {
                    id: this.id,
                }
            });
        }
    },
    mounted() {
        this.getSection();
    }
}
</script>
<style scoped lang="scss">
.fas {
    width: 24;
    // font-size: 24;
    // vertical-align: center;
}

.action-bar-title {
    font-size: 20;
    // text-align: center;
    // vertical-align: center;
}

.left-aligned {
    align-items: left;
}

.img {
    width: 50%;
    align-items: center;
}

.label {
    font-weight: 700;
    padding: 2;
    margin: 2;
}

.text-field {
    margin-bottom: 10;
    color: #000;
    font-size: 16;
}

.mb-20 {
    margin-bottom: 20;
}

.borderr {
    padding: 10;
    border: 1px solid #000;
    border-radius: 5;
}

.borderr.active {
    padding: 10;
    border-radius: 5;
    background-color: rgba(0, 255, 213, 0.227);
}
</style>

