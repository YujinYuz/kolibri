<template>

  <UiToolbar
    :title="appBarTitle"
    textColor="white"
    type="clear"
    :showIcon="showIcon"
    :style="{
      height: height + 'px',
      backgroundColor: primary ? $themeTokens.primary : $themeTokens.text,
    }"
    @nav-icon-click="$emit('navIconClick')"
  >
    <router-link
      v-if="hasRoute"
      slot="icon"
      :to="route"
      :class="['link', $computedClass(linkStyle)]"
    >
      <!-- TODO add aria label? -->
      <UiIconButton
        type="flat"
        class="icon"
        :style="{fill: $themeTokens.textInverted}"
        @click="$emit('navIconClick')"
      >
        <mat-svg
          v-if="icon === 'close'"
          name="close"
          category="navigation"
        />
        <mat-svg
          v-else-if="icon === 'arrow_back' && !isRtl"
          name="arrow_back"
          category="navigation"
        />
        <mat-svg
          v-else-if="icon === 'arrow_back' && isRtl"
          name="arrow_forward"
          category="navigation"
        />
      </UiIconButton>
    </router-link>

    <UiIconButton
      v-else
      type="flat"
      class="icon"
      :style="{fill: $themeTokens.textInverted}"
      @click="$emit('navIconClick')"
    >
      <mat-svg
        v-if="icon === 'close'"
        name="close"
        category="navigation"
      />
      <mat-svg
        v-if="icon === 'arrow_back' && !isRtl"
        name="arrow_back"
        category="navigation"
      />
      <mat-svg
        v-if="icon === 'arrow_back' && isRtl"
        name="arrow_forward"
        category="navigation"
      />
    </UiIconButton>
  </UiToolbar>

</template>


<script>

  import UiToolbar from 'kolibri.coreVue.components.UiToolbar';
  import UiIconButton from 'kolibri.coreVue.components.UiIconButton';
  import { validateLinkObject } from 'kolibri.utils.validators';

  export default {
    name: 'ImmersiveToolbar',
    components: {
      UiToolbar,
      UiIconButton,
    },
    props: {
      appBarTitle: {
        type: String,
        required: true,
      },
      height: {
        type: Number,
        required: true,
      },
      icon: {
        type: String,
        required: false,
        default: 'close',
        validator(val) {
          return ['close', 'arrow_back'].includes(val);
        },
      },
      showIcon: {
        type: Boolean,
        required: false,
        default: true,
      },
      route: {
        type: Object,
        required: false,
        validator: validateLinkObject,
      },
      primary: {
        type: Boolean,
        required: false,
        default: true,
      },
    },
    computed: {
      hasRoute() {
        return Boolean(this.route);
      },
      linkStyle() {
        const hoverAndFocus = {
          backgroundColor: this.primary ? this.$themeTokens.primaryDark : this.$themePalette.black,
        };
        return {
          backgroundColor: this.primary ? this.$themeTokens.primary : this.$themeTokens.text,
          ':hover': hoverAndFocus,
        };
      },
    },
  };

</script>


<style lang="scss" scoped>

  // only used when using a link. Otherwise, uses UiToolbar's styles
  .icon {
    // copied from keen
    width: 3em;
    height: 3em;
  }

  .link {
    display: inline-block;
    border-radius: 50%;
  }

</style>
