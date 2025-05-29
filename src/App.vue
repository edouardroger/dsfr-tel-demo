<script lang="ts">
import { defineComponent, ref, nextTick } from 'vue';
import DsfrTel from 'dsfr-tel';

export default defineComponent({
  name: 'App',
  components: {
    DsfrTel
  },
  setup() {
    const phoneInput = ref<typeof DsfrTel | null>(null);
    const phoneValidCard = ref<HTMLDivElement | null>(null);
    const isPhoneNumberValid = ref(false);
    const phoneNumbers = ref<{ [key: string]: string }>({});
    const phoneType = ref('');

    const validatePhone = async (e: Event) => {
      e.preventDefault();
      if (!phoneInput.value) return;

      const isValid = phoneInput.value.validatePhoneNumber();
      if (isValid) {
        isPhoneNumberValid.value = true;
        phoneType.value = phoneInput.value.getPhoneNumberType(); // Récupère le type du numéro
        phoneNumbers.value = {
          national: phoneInput.value.getPhoneNumberFormatted('NATIONAL'),
          e164: phoneInput.value.getPhoneNumberFormatted('E164'),
          international: phoneInput.value.getPhoneNumberFormatted('INTERNATIONAL'),
          rfc3966: phoneInput.value.getPhoneNumberFormatted('RFC3966')
        };
        // Attendre que le DOM soit mis à jour
        await nextTick();
        // Mettre le focus sur le conteneur de résultat
        phoneValidCard.value?.focus();
      } else {
        isPhoneNumberValid.value = false;
        phoneNumbers.value = {};
        phoneType.value = '';
        if (phoneInput.value.focus) {
          phoneInput.value.focus();
        } else {
          const inputEl = phoneInput.value.$el.querySelector('input');
          inputEl && inputEl.focus();
        }
      }
    };

    return {
      phoneInput,
      phoneValidCard,
      isPhoneNumberValid,
      phoneNumbers,
      phoneType,
      validatePhone
    };
  }
});
</script>
<template>
  <main class="fr-container" role="main">
    <div class="fr-grid-row fr-grid-row--center fr-grid-row--middle" style="min-height: 75vh;">
      <div class="fr-col-12 fr-col-lg-5 fr-col-offset-lg-1">
        <h1 class="fr-h4">Démonstration du paquet dsfr-tel</h1>
        <form @submit="validatePhone">
          <DsfrTel ref="phoneInput" fieldsetLegend="Votre numéro de téléphone portable" required />
          <div class="fr-mt-2w">
            <button type="submit" class="fr-btn fr-btn--sm">Vérifier le numéro</button>
          </div>
        </form>

        <div class="fr-mt-2w fr-alert fr-alert--success" v-if="isPhoneNumberValid" tabindex="-1" ref="phoneValidCard">
          <p>
            Numéro valide <span v-if="phoneType"> (type : {{ phoneType }})</span> au format :
          </p>
          <ul>
            <li v-for="(formatted, format) in phoneNumbers" :key="format">
              <span class="fr-text--bold">{{String(format).charAt(0).toUpperCase() + String(format).slice(1) }}</span> : {{ formatted }}
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="fr-text-align-center fr-mt-3w">
      <p>
        <a href="https://github.com/edouardroger/dsfr-tel" class="fr-link" target="_blank" rel="noopener noreferrer">
          Informations sur le paquet
        </a>
      </p>
    </div>
  </main>
</template>

<style scoped>
@import 'https://cdn.jsdelivr.net/npm/@gouvfr/dsfr@1.13.1/dist/dsfr.min.css';
</style>