<template>
    <div class="card shadow-card">
        <FormLabel>
            {{ $t('subscription') }}
        </FormLabel>

        <b class="-mt-3 mb-0.5 block text-xl font-extrabold sm:text-3xl">
            {{ status }}
        </b>

        <b class="mb-3 mb-8 block text-sm dark:text-gray-500 text-gray-400">
            {{ subscription.relationships.plan.data.attributes.name }} /
            {{ price }}
        </b>

        <div v-for="(limit, i) in limitations" :key="i" :class="{ 'mb-6': Object.keys(limitations).length - 1 !== i }">
            <b class="mb-3 block text-sm dark:text-gray-500 text-gray-400">
                {{ limit.message }}
            </b>
            <ProgressLine v-if="limit.isVisibleBar" :data="limit.distribution" />
        </div>
    </div>
</template>
<script>
import FormLabel from '../../../../components/Others/Forms/FormLabel'
import ProgressLine from '../../../../components/Admin/ProgressLine'
import { mapGetters } from 'vuex'

export default {
    name: 'UserFixedSubscription',
    props: ['subscription', 'user'],
    components: {
        ProgressLine,
        FormLabel,
    },
    computed: {
        status() {
            return {
                active: this.$t('active_until', {date: this.subscription.attributes.renews_at}),
                cancelled: this.$t('ends_at_date', {date: this.subscription.attributes.ends_at}),
            }[this.subscription.attributes.status]
        },
        price() {
            return {
                month: this.$t('price_per_month', {price: this.subscription.relationships.plan.data.attributes.price}),
                year: this.$t('price_per_year', {price: this.subscription.relationships.plan.data.attributes.price}),
            }[this.subscription.relationships.plan.data.attributes.interval]
        },
    },
    data() {
        return {
            limitations: [],
        }
    },
    created() {
        Object.entries(this.user.data.meta.limitations).map(([key, item]) => {
            let payload = {
                color: {
                    max_storage_amount: 'warning',
                    max_team_members: 'purple',
                },
                message: {
					max_storage_amount: this.$t('total_x_of_x_used', {use: item.use, total:item.total }),
					max_team_members: item.total === -1
						? this.$t('max_team_members.unlimited')
						: this.$t('total_x_of_x_members', {use: item.use, total:item.total }),
                },
                title: {
					max_storage_amount: this.$t('storage'),
					max_team_members: this.$t('team_members'),
                },
            }

            this.limitations.push({
                message: payload.message[key],
				isVisibleBar: item.total === -1,
                distribution: [
                    {
                        progress: item.percentage,
                        color: payload.color[key],
                        title: payload.title[key],
                    },
                ],
            })
        })
    },
}
</script>
