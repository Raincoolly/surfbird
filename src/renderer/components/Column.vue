<template>
  <section class="c-column">
    <div class="c-column__holder">
      <div class="c-column__panel">
        <column-header :data="data"></column-header>
        <column-content></column-content>
      </div>
    </div>
  </section>
</template>

<script>
import ColumnHeader from './Column/Header'
import ColumnContent from './Column/Content'

export default {
  props: ['data', 'index'],
  created: function () {
    let payload = {index: this.index, type: this.data.type, profile: this.$store.state.profiles.activeProfile, owner: this.data.owner}
    this.$store.dispatch('startStreaming', payload)
  },
  components: {
    ColumnHeader,
    ColumnContent
  },
  provide: function () {
    let provider = {}

    Object.defineProperty(provider, 'columnIndex', {
      enumerable: true,
      get: () => this.index
    })

    Object.defineProperty(provider, 'columnOwner', {
      enumerable: true,
      get: () => this.data.owner
    })

    Object.defineProperty(provider, 'columnProfile', {
      enumerable: true,
      get: () => this.$store.state.profiles.activeProfile
    })

    return provider
  },
  name: 'column'
}
</script>

<style lang="scss">
.c-column {
  position: relative;
  z-index: 10;
  display: inline-block;
  overflow: hidden;
  width: 433px;
  height: 100%;
  background-color: #fff;
  font-size: 1rem;
  white-space: normal;

  &__holder {
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  &__panel {
    display: flex;
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;
  }
}
</style>
