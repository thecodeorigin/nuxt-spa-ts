<template>
  <!-- Checkout the variants customization bootstrap-vue/variants folder -->
  <b-navbar toggleable="lg" type="dark" variant="purple">
    <b-navbar-brand>
      <nuxt-link to="/" style="text-decoration: none">
        {{ $t('home.navbar') }}
      </nuxt-link>
    </b-navbar-brand>

    <b-navbar-toggle target="nav-collapse" />

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <nuxt-link to="/introduction" class="text-light">
          Introduction
        </nuxt-link>
        <nuxt-link to="/secret" class="text-light ml-3">
          Secret page
        </nuxt-link>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <!-- Fix DOM mismatch between client-side and server-side -->
        <client-only>
          <!-- Login button -->
          <b-button
            v-if="auth && $route.path != '/login'"
            size="sm"
            class="mr-3"
            style="border-radius: 2rem"
            variant="danger"
            :title="$t('navbar.logout')"
            @click="$store.dispatch('root/logout')"
          >
            <fa class="text-light" :icon="['fas', 'sign-in-alt']" />
          </b-button>
          <b-button
            v-if="!auth && $route.path != '/login'"
            size="sm"
            class="mr-3"
            style="border-radius: 2rem"
            variant="success"
            :title="$t('navbar.login')"
            @click="
              $root.$emit(
                'bv::show::modal',
                'sign-in-modal',
                '#focusThisOnClose'
              )
            "
          >
            <fa class="text-light" :icon="['fas', 'sign-in-alt']" />
          </b-button>
          <!-- i18n button -->
          <b-button
            v-if="locale == 'en'"
            size="sm"
            style="border-radius: 2rem"
            variant="danger"
            @click="changeLanguage('vi')"
          >
            <fa class="text-warning" :icon="['fas', 'globe-asia']" />
          </b-button>
          <b-button
            v-if="locale == 'vi'"
            size="sm"
            style="border-radius: 2rem"
            variant="primary"
            @click="changeLanguage('en')"
          >
            <fa :icon="['fas', 'globe-asia']" />
          </b-button>
        </client-only>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Mutation, State } from 'nuxt-property-decorator'
import { RootMutations, IAuth } from '~/typings'
@Component({})
export default class Navbar extends Vue {
  // Init $i18n property // VS Code warning
  $i18n!: any
  @State('locale') locale!: string
  @State('auth') auth!: IAuth
  // Mutation mapping with type check for payload:
  @Mutation(RootMutations.SET_LANG) SET_LANG!: (key: string) => void
  changeLanguage(locale: string) {
    this.SET_LANG(locale) // store in state and localStorage
    this.$i18n.locale = locale // live change on website
  }
}
</script>
