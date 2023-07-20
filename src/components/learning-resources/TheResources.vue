<template>
  <base-card>
    <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode"
      >Stored Resources</base-button
    >
    <base-button @click="setSelectedTab('add-resource')" :mode="addResButtonMode"
      >Add Resource</base-button
    >
  </base-card>
  <KeepAlive>
    <component :is="selectedTab"></component>
  </KeepAlive>
</template>

<script>
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

export default {
  components: {
    StoredResources,
    AddResource
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedRessources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation.',
          link: 'https://vuejs.org'
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://google.com'
        }
      ]
    }
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat'
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat'
    }
  },
  provide() {
    return {
      resources: this.storedRessources,
      addResource: this.addResource,
      deleteResource: this.removeResource
    }
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab
    },
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString,
        title: title,
        description: description,
        link: url
      }
      this.storedRessources.unshift(newResource)
      this.selectedTab = 'stored-resources'
    },
    removeResource(resId) {
      const resIndex = this.storedRessources.findIndex((res) => res.id === resId)
      this.storedRessources.splice(resIndex, 1)
    }
  }
}
</script>
