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
    const phoneNumber = ref('');

    const validatePhone = () => {
      if (phoneInput.value) {
        const isValid = phoneInput.value.validatePhoneNumber();
        if (isValid) {
          isPhoneNumberValid.value = true;
          phoneNumber.value = phoneInput.value.getPhoneNumberFormatted();
        } else {
          isPhoneNumberValid.value = false;
        }
      }
    };

    return {
      phoneInput,
      isPhoneNumberValid,
      phoneNumber,
      validatePhone
    };
  }
});
</script>

<template>
  <div class="fr-container fr-p-5w">
    <div class="fr-grid-row fr-grid-row--center fr-grid-row--middle" style="height: 100vh;">
      <div class="fr-col-5 fr-col-offset-1">
        <h1 class="fr-h4">Démonstration du paquet dsfr-tel</h1>

        <DsfrTel ref="phoneInput" label="Votre numéro de téléphone portable" hint="Au format national" />
        
        <div class="fr-mt-2w">
          <button @click="validatePhone" class="fr-btn fr-btn--sm">Vérifier le numéro</button>
        </div>

        <div class="fr-mt-2w">
          <p v-if="isPhoneNumberValid" class="fr-text-align-center">
            Numéro de téléphone valide : {{ phoneNumber }}
          </p>
        </div>
      </div>
    </div>

    <div class="fr-text-align-center fr-mt-3w">
      <p><a href="https://github.com/edouardroger/dsfr-tel" class="fr-link">Informations sur le paquet</a></p>
    </div>
  </div>
</template>

<style scoped>
@import 'https://cdn.jsdelivr.net/npm/@gouvfr/dsfr@1.12.1/dist/dsfr.min.css';
</style>
