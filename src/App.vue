<script lang="ts">
import { defineComponent, ref } from 'vue';
import DsfrTel from 'dsfr-tel';

export default defineComponent({
  name: 'App',
  components: {
    DsfrTel
  },
  setup() {
    const phoneInput = ref<typeof DsfrTel | null>(null);
    const isPhoneNumberValid = ref(false);
    const phoneNumbers = ref<{ [key: string]: string }>({});

    const validatePhone = (e: Event) => {
      e.preventDefault();
      if (phoneInput.value) {
        const isValid = phoneInput.value.validatePhoneNumber();
        if (isValid) {
          isPhoneNumberValid.value = true;
          phoneNumbers.value = {
            national: phoneInput.value.getPhoneNumberFormatted('NATIONAL'),
            e164: phoneInput.value.getPhoneNumberFormatted('E164'),
            international: phoneInput.value.getPhoneNumberFormatted('INTERNATIONAL'),
            rfc3966: phoneInput.value.getPhoneNumberFormatted('RFC3966')
          };
        } else {
          isPhoneNumberValid.value = false;
          phoneNumbers.value = {};
        }
      }
    };

    return {
      phoneInput,
      isPhoneNumberValid,
      phoneNumbers,
      validatePhone
    };
  }
});
</script>
<template>
  <div class="fr-container">
    <div class="fr-grid-row fr-grid-row--center fr-grid-row--middle" style="min-height: 75vh;">
      <div class="fr-col-12 fr-col-lg-5 fr-col-offset-lg-1">
        <h1 class="fr-h4">Démonstration du paquet dsfr-tel</h1>
        <form @submit="validatePhone">
          <DsfrTel ref="phoneInput" fieldsetLegend="Votre numéro de téléphone portable" />

          <div class="fr-mt-2w">
            <button type="submit" class="fr-btn fr-btn--sm">Vérifier le numéro</button>
          </div>
        </form>

        <div class="fr-mt-2w fr-card fr-p-3w" v-if="isPhoneNumberValid">
          <p>Numéro valide au format :</p>
          <ul>
            <li v-for="(formatted, format) in phoneNumbers" :key="format">
              {{ format }} : {{ formatted }}
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="fr-text-align-center fr-mt-3w">
      <p><a href="https://github.com/edouardroger/dsfr-tel" class="fr-link">Informations sur le paquet</a></p>
    </div>
  </div>
</template>

<style scoped>
@import 'https://cdn.jsdelivr.net/npm/@gouvfr/dsfr@1.13.1/dist/dsfr.min.css';
</style>