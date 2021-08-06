<template>
  <div>
    <Header />
    <AddLocationForm />
    <FCardLay :f_cards="f_cards" />
  </div>
</template>

<script>
import Header from '@/components/Header'
import AddLocationForm from '../components/AddLocationForm'
import FCardLay from '@/components/FCardLay'
export default {
  components: { Header, AddLocationForm, FCardLay },
  data: function () {
    return {
      f_cards: [
        {
          location: 'Mykolaiv',
          temperature: '+33',
        },
        {
          location: 'Kyiv',
          temperature: '+28',
        },
        {
          location: 'Kyiv',
          temperature: '+28',
        },
        {
          location: 'Kyiv',
          temperature: '+28',
        },
        {
          location: 'Kyiv',
          temperature: '+28',
        },
        {
          location: 'Kyiv',
          temperature: '+28',
        },
      ],
    }
  },
}
</script>
