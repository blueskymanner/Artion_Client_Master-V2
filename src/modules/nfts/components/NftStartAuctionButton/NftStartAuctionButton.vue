<template>
    <div class="nftstartauctionbutton">
        <a-button
            :label="$t('nftstartauctionbutton.startAuction')"
            :loading="txStatus === 'pending'"
            @click.native="onButtonClick"
        />

        <a-tx-window
            ref="window"
            :title="$t('nftstartauctionwindow.title')"
            v-slot="{ onTxStatus }"
            class="fwindow-width-5"
        >
            <nft-auction-form
                :token="token"
                @transaction-status="
                    onTxStatus($event);
                    onTransactionStatus($event);
                "
            />
        </a-tx-window>
    </div>
</template>

<script>
import AButton from '@/common/components/AButton/AButton.vue';
import ATxWindow from '@/common/components/ATxWindow/ATxWindow.vue';
import NftAuctionForm from '@/modules/nfts/components/NftAuctionForm/NftAuctionForm.vue';

export default {
    name: 'NftStartAuctionButton',

    components: { NftAuctionForm, ATxWindow, AButton },

    props: {
        token: {
            type: Object,
            default() {
                return {};
            },
        },
    },

    data() {
        return {
            txStatus: '',
        };
    },

    methods: {
        onButtonClick() {
            this.$refs.window.show();
        },

        onTransactionStatus(payload) {
            this.txStatus = payload.status;

            if (this.txStatus === 'success') {
                this.$emit('tx-success');
            }
        },
    },
};
</script>
