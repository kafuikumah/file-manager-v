<template>
    <AuthContentWrapper ref="auth" class="h-screen">
        <AuthContent :visible="true">
            <Headline
                :title="$t('temporary_unavailable')"
                :description="$t('shared_link_temporary_unavailable')"
            />

            <span class="additional-link"
                >{{ $t('page_registration.have_an_account') }}
                <router-link :to="{ name: 'SignIn' }" class="text-theme font-bold">
                    {{ $t('log_in') }}
                </router-link>
            </span>
        </AuthContent>
    </AuthContentWrapper>
</template>

<script>
import AuthContentWrapper from '../components/Auth/AuthContentWrapper'
import AuthContent from '../components/Auth/AuthContent'
import Headline from './Auth/Headline'

export default {
    name: 'TemporaryUnavailable',
    components: {
        AuthContentWrapper,
        AuthContent,
        Headline,
    },
}
</script>
