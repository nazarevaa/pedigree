<template>
  <PageLayout>
    <ScrollingPanel :sections="sections" />
    <NavigationPanel />
    <section class="p-16">
      <PersonCard :person="person" ref="PersonCard" />
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
  data() {
    return {
      sections: [
        { id: 'info-section', title: 'Общая информация', chapter: true },
        { id: 'parents-section', title: 'Родители',chapter: true },
        { id: 'childs-section', title: 'Дети', chapter: true },
      ]
    }
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
    }
  }, 
  mounted () {
    const sections = []

    sections.push({ id: 'education-section', title: 'Образование', chapter: true })
    const amountEducationItems = this.$refs.PersonCard.amountEducationItems()
    for(let ind = 0; ind < amountEducationItems; ind++) {
      sections.push({ id: 'education-section' + ind, title: 'Образование ' + (ind + 1), chapter: false })
    }

    sections.push({ id: 'weddings-section', title: 'Брачные союзы', chapter: true })
    const amountWeddingItems = this.$refs.PersonCard.amountWeddingItems()
    for(let ind = 0; ind < amountWeddingItems; ind++) {
      sections.push({ id: 'weddings-section' + ind, title: 'Брачный союз ' + (ind + 1), chapter: false })
    }

    sections.push({ id: 'military-section', title: 'Военная служба', chapter: true } )
    const amountMilitaryItems = this.$refs.PersonCard.amountMilitaryItems()
    for(let ind = 0; ind < amountMilitaryItems; ind++) {
      sections.push({ id: 'military-section' + ind, title: 'Военная служба ' + (ind + 1), chapter: false })
    }

    this.sections = [...this.sections, ...sections];
  }
}
</script>
