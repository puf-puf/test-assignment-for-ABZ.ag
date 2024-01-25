<template>
  <a :href="`tel:${phone}`">{{ formatedNumber }}</a>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps(['phone'])
const formatedNumber = ref(formatNumber(props.phone, true))

function formatNumber(phoneNumber, plusSign) {
  if (plusSign === undefined) return 'Plus Sign not specified'
  if (phoneNumber.length < 12 || phoneNumber.length > 13) return 'Wrong Phone Number size'
  const countryCode = plusSign ? phoneNumber.substring(0, 3) : phoneNumber.substring(0, 2)
  const firstNumbers = plusSign ? phoneNumber.substring(3, 6) : phoneNumber.substring(2, 5)
  const secondNumbers = plusSign ? phoneNumber.substring(6, 9) : phoneNumber.substring(5, 8)
  const thirdNumbers = plusSign ? phoneNumber.substring(9, 11) : phoneNumber.substring(8, 10)
  const fourthNumbers = plusSign ? phoneNumber.substring(11, 13) : phoneNumber.substring(10, 12)

  const formattedString = `${
    plusSign ? '' : '+'
  }${countryCode} (${firstNumbers}) ${secondNumbers} ${thirdNumbers} ${fourthNumbers}`
  return formattedString
}
</script>
