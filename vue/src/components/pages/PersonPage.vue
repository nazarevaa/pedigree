<template>
  <PageLayout>
    <ScrollingPanel :sections="sections" />
    <NavigationPanel />
    <section class="p-16">
      <PersonCard :person="person" />
    </section>
  </PageLayout>
</template>

<script>
import { mapGetters } from 'vuex'
import PageLayout from '@/components/parts/PageLayout.vue'
import PersonCard from '@/components/cards/PersonCard.vue'
import NavigationPanel from '@/components/ui/NavigationPanel.vue'
import ScrollingPanel from '@/components/ui/ScrollingPanel.vue'
import { emptyPerson } from '@/services/person'

export default {
  name: 'PersonPage',
  components: {
    PageLayout,
    PersonCard,
    NavigationPanel,
    ScrollingPanel
  },
  computed: {
    ...mapGetters('persons', [
      'getPersonById'
    ]),
    person () {
      if (this.getPersonById(this.id)) {
        return this.getPersonById(this.id)
      }
      return emptyPerson()
    },
    id () {
      return this.$route.params.id
    },
    sections () {
      return [
        { id: 'info-section', title: 'Общая информация', subSection: this.generateSubsection('info', 'info-section','Информация')},
        { id: 'parents-section', title: 'Родители', subSection: this.generateSubsection('parents', 'parent-section','Родитель') },
        { id: 'childs-section', title: 'Дети', subSection: this.generateSubsection('children', 'child-section','Ребенок') },
        { id: 'weddings-section', title: 'Брачные союзы', subSection: this.generateSubsection('weddings', 'weddings-section', 'Брачный союз') },
        { id: 'military-section', title: 'Военная служба', subSection: this.generateSubsection('militaries', 'military-section', 'Военная служба') },
        { id: 'education-section', title: 'Образование', subSection: this.generateSubsection('educations', 'education-section', 'Образование') }
      ]
    }
  },
  methods: {
    generateSubsection (objectName, sectionName, title) {
      if (this.person[objectName]) {
        return this.person[objectName].map((item, index) => ({
          id: `${sectionName}${index}`,
          title: `${title} ${index + 1}`
        }))
      }
      return []
    }
  }
}
</script>
