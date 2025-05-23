<template>

  <div class="content-wrapper">
    <SideNav
      :class="{ show: isSideNavVisible, 'side-nav': true }"
      @update-side-nav="handleSideNavUpdate"
    />

    <div
      v-if="isSideNavVisible"
      class="overlay"
      @click="handleSideNavUpdate(false)"
    ></div>

    <Header
      :sections="pageSections"
      :title="page.title"
      :codeStyle="page.isCode"
      class="floating-header"
      @update-side-nav="handleSideNavUpdate"
    />

    <div class="border">
      <!-- second header used as a spacer -->
      <Header
        :sections="pageSections"
        :title="page.title"
        :codeStyle="page.isCode"
        style="visibility: hidden"
      />

      <div class="content">
        <slot></slot>
        <DocsPageSection
          v-for="(section, i) in apiSections"
          :key="i"
          :title="section.title"
          :anchor="section.anchor"
          fullwidth
        >
          <component
            :is="section.component"
            :api="api[section.key]"
          />
        </DocsPageSection>
      </div>
    </div>
  </div>

</template>


<script>

  import consola from 'consola';
  import DocsPageSection from '../DocsPageSection';
  import SlotsTable from './jsdocs/SlotsTable';
  import EventsTable from './jsdocs/EventsTable';
  import PropsTable from './jsdocs/PropsTable';
  import MethodsTable from './jsdocs/MethodsTable';
  import Header from './Header';
  import SideNav from './SideNav';
  import jsdocs from '~/jsdocs.js';
  import tableOfContents from '~/tableOfContents.js';

  function sectionsForDefaultSlot(children) {
    if (children === undefined) {
      return [];
    }
    return children
      .filter(
        node =>
          node.componentOptions &&
          node.componentOptions.tag === DocsPageSection.name &&
          node.componentOptions.propsData.anchor,
      )
      .map(node => node.componentOptions.propsData);
  }

  export default {
    name: 'DocsPageTemplate',
    components: {
      Header,
      SideNav,
      PropsTable,
      SlotsTable,
      EventsTable,
      MethodsTable,
    },
    props: {
      apiDocs: {
        type: Boolean,
        default: false,
      },
    },
    data() {
      return {
        isSideNavVisible: false, // Track state received from child
      };
    },
    computed: {
      page() {
        let path = this.$route.path;
        // clear trailing slashes, which is necessary on netlify but not locally
        if (path !== '/') {
          path = path.replace(/\/$/, '');
        }
        // search for page
        for (const section of tableOfContents) {
          for (const page of section.pages) {
            if (path === page.path) {
              return page;
            }
          }
        }
        // page not found
        consola.error(`'${path}' not found`);
        return undefined;
      },
      pageSections() {
        // look at children for sections and extract links
        const pageSections = sectionsForDefaultSlot(this.$slots.default);
        // add any applicable API sections
        this.apiSections.forEach(section => pageSections.push(section));
        return pageSections;
      },
      fullTitle() {
        const main = 'Kolibri Design System';
        return this.page.title ? `${this.page.title} - ${main}` : main;
      },
      api() {
        if (!this.apiDocs) return {};
        return jsdocs[this.page.title];
      },
      apiSections() {
        const sections = [];
        if (this.api && this.api.props && this.api.props.length) {
          sections.push({
            key: 'props',
            anchor: '#props',
            title: 'Props',
            component: PropsTable,
          });
        }
        if (this.api && this.api.events && this.api.events.length) {
          sections.push({
            key: 'events',
            anchor: '#events',
            title: 'Events',
            component: EventsTable,
          });
        }
        if (this.api && this.api.slots && this.api.slots.length) {
          sections.push({
            key: 'slots',
            anchor: '#slots',
            title: 'Slots',
            component: SlotsTable,
          });
        }
        if (this.api && this.api.methods && this.api.methods.length) {
          sections.push({
            key: 'methods',
            anchor: '#methods',
            title: 'Methods',
            component: MethodsTable,
          });
        }
        return sections;
      },
    },
    methods: {
      handleSideNavUpdate(visible) {
        this.isSideNavVisible = visible; // Update parent state
      },
    },
    head() {
      return {
        title: this.fullTitle,
      };
    },
  };

</script>


<style lang="scss" scoped>

  @import '~/assets/definitions';
  @import '../../../lib/styles/definitions';

  .floating-header {
    position: fixed;
    top: 0;
    right: 0;
    left: $nav-width;
    z-index: 95;
  }

  @media screen and (max-width: 768px) {
    .floating-header {
      left: 0;
    }
  }

  .side-nav {
    transform: translateX(0);
    @extend %dropshadow-2dp;
  }

  @media (max-width: 768px) {
    .side-nav {
      transform: translateX(-100%);
    }

    .side-nav.show {
      transform: translateX(0);
    }
  }

  .content-wrapper {
    margin-left: $nav-width;
  }

  @media (max-width: 768px) {
    .content-wrapper {
      margin-left: 0;
    }
  }

  .border {
    border-left: 1px solid $border-color;
  }

  .content {
    padding-right: 32px;
    padding-bottom: 400px;
    padding-left: 32px;
  }

  .overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 98;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.7);
  }

</style>
