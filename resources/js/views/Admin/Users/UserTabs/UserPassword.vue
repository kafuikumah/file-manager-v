<template>
    <PageTab>
        <div class="card shadow-card">
            <FormLabel>
                {{ $t('user_box_password.title') }}
            </FormLabel>

            <AppInputText
                :title="$t('reset_user_password')"
                :description="$t('user_box_password.description')"
                :is-last="true"
            >
                <ButtonBase
                    @click.native="requestPasswordResetEmail"
                    :loading="isSendingRequest"
                    :disabled="isSendingRequest"
                    class="w-full sm:w-auto"
                    button-style="theme"
                >
                    {{ $t('admin_page_user.send_password_link') }}
                </ButtonBase>
            </AppInputText>
        </div>
    </PageTab>
</template>

<script>
import { ValidationProvider, ValidationObserver } from 'vee-validate/dist/vee-validate.full'
import PageTabGroup from '../../../../components/Others/Layout/PageTabGroup'
import AppInputSwitch from '../../../../components/Admin/AppInputSwitch'
import FormLabel from '../../../../components/Others/Forms/FormLabel'
import ButtonBase from '../../../../components/FilesView/ButtonBase'
import SetupBox from '../../../../components/Others/Forms/SetupBox'
import PageTab from '../../../../components/Others/Layout/PageTab'
import InfoBox from '../../../../components/Others/Forms/InfoBox'
import { required } from 'vee-validate/dist/rules'
import { events } from '../../../../bus'
import axios from 'axios'
import AppInputText from '../../../../components/Admin/AppInputText'

export default {
    name: 'UserPassword',
    components: {
        AppInputText,
        ValidationProvider,
        ValidationObserver,
        AppInputSwitch,
        PageTabGroup,
        ButtonBase,
        FormLabel,
        SetupBox,
        required,
        InfoBox,
        PageTab,
    },
    data() {
        return {
            isLoading: false,
            isSendingRequest: false,
        }
    },
    methods: {
        requestPasswordResetEmail() {
            this.isSendingRequest = true

            axios
                .post(`${this.$store.getters.api}/admin/users/${this.$route.params.id}/reset-password`, {})
                .then(() => {
                    events.$emit('toaster', {
                        type: 'success',
                        message: this.$t('toaster.sended_password'),
                    })
                })
                .catch(() => {
                    events.$emit('alert:open', {
                        title: this.$t('popup_error.title'),
                        message: this.$t('popup_error.message'),
                    })
                })
                .finally(() => (this.isSendingRequest = false))
        },
    },
}
</script>
