<template>
  <teleport to="body">
    <CountryIntlModal
      :countryIntlModalIsActive="countryIntlModalIsActive"
      @sendCountrySelect="getCountrySelect"
      @sendCloseModal="getCloseModal"
    />
  </teleport>
  <div class="country-intl-input-group">
    <div class="country-intl-input-title">Phone Number</div>
    <div class="country-intl-input-wrap">
      <button type="button" @click="openIntlModal()" class="country-intl-tel-btn">
        <img class="intl-flag" :class="intlInfo.alpha2Code" />

        <span class="country-intl-dial-code"> +{{ intlInfo.dialCode }} </span>
      </button>

      <input
        type="text"
        class="country-intl-form-control"
        placeholder="Phone number"
        autocomplete="off"
        v-model="phoneNumberValue"
        @focus="focused = true"
        maxlength="13"
      />

      <button
        type="button"
        class="country-intl-input-clear-btn"
        @click="clearPhoneValue()"
        tabindex="-1"
        v-show="phoneNumberValue.length > 0"
      >
        reset
        <i class="ico ico-reset"></i>
      </button>
    </div>
    <p class="country-intl-input-feed" v-show="focused">{{ props?.inputInfoMsg }}</p>
  </div>
</template>

<script setup lang="ts">
import { watch, ref } from 'vue';
import CountryIntlModal from './CountryIntlModal.vue';

const props = defineProps({
  firstValue: { type: Object },
  inputInfoMsg: { type: String },
});

const emit = defineEmits(['sendIntlModalIsActive', 'sendInputValue']);

const countryIntlModalIsActive = ref(false);

const focused = ref(false);
const phoneNumberValue = ref('');

const intlInfo = ref(
  props.firstValue
    ? {
        alpha2Code: props.firstValue.alpha2Code,
        country: props.firstValue?.country,
        dialCode: props.firstValue.dialCode,
      }
    : {
        alpha2Code: 'us',
        country: 'United States',
        dialCode: '1',
      },
);

const clearPhoneValue = () => {
  phoneNumberValue.value = '';
};

const getCountrySelect = (e: any) => {
  intlInfo.value = e;
  console.log(intlInfo.value);
};

const getCloseModal = (e: any) => {
  countryIntlModalIsActive.value = e;
  emit('sendIntlModalIsActive', e);
};

const openIntlModal = () => {
  countryIntlModalIsActive.value = !countryIntlModalIsActive.value;
  emit('sendIntlModalIsActive', true);
};

let phoneNumberCopy = '';
watch(phoneNumberValue, () => {
  if (phoneNumberValue.value.match(/[^0-9]/) !== null && phoneNumberValue.value.length < 14) {
    phoneNumberValue.value = phoneNumberCopy;
  } else {
    phoneNumberCopy = phoneNumberValue.value;
  }

  const alpha2Code = intlInfo.value.alpha2Code;
  const dialCode = intlInfo.value.dialCode;
  const countryName = intlInfo.value?.country;
  const phoneNumber = phoneNumberValue.value;

  emit('sendInputValue', { alpha2Code, dialCode, countryName, phoneNumber });
});
</script>

<style lang="scss">
.country-intl-input-group {
  * {
    &:focus,
    &:hover,
    &:active,
    &:target {
      outline: none;
    }
  }
}

.country-intl-input-title {
  text-align: left;
}

.country-intl-input-wrap {
  display: flex;
  position: relative;
}

.country-intl-tel-btn {
  max-width: 100px;
  width: 100%;
  height: 37px;
  border-radius: 0;
  margin-right: 7px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
}

.country-intl-dial-code {
  margin-left: 5px;
}

.country-intl-form-control {
  width: 100%;
  border: none;
  padding-left: 5px;
}

.country-intl-input-clear-btn {
  width: 37px;
  height: 37px;
  position: absolute;
  background-size: 12px;
  background-repeat: no-repeat;
  background-position: 50%;
  font-size: 0;
  top: 0;
  right: 0;
  cursor: pointer;
  background-color: inherit;
  background-repeat: no-repeat;
  background-image: url('data:image/svg+xml,<svg viewBox="0 0 30 30" fill="white" xmlns="http://www.w3.org/2000/svg"><path d="M6.6129 5.2097L6.70711 5.29289L15 13.585L23.2929 5.29289C23.6834 4.90237 24.3166 4.90237 24.7071 5.29289C25.0676 5.65338 25.0953 6.22061 24.7903 6.6129L24.7071 6.70711L16.415 15L24.7071 23.2929C25.0976 23.6834 25.0976 24.3166 24.7071 24.7071C24.3466 25.0676 23.7794 25.0953 23.3871 24.7903L23.2929 24.7071L15 16.415L6.70711 24.7071C6.31658 25.0976 5.68342 25.0976 5.29289 24.7071C4.93241 24.3466 4.90468 23.7794 5.2097 23.3871L5.29289 23.2929L13.585 15L5.29289 6.70711C4.90237 6.31658 4.90237 5.68342 5.29289 5.29289C5.62334 4.96245 6.12751 4.91161 6.5114 5.14038L6.6129 5.2097Z"/></svg>');
}

.country-intl-input-feed {
  margin: 0;
  padding-top: 2px;
  padding-left: 2px;
  text-align: left;
}
</style>
