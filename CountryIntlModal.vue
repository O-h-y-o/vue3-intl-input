<template>
  <div class="country-intl-modal" :class="{ show: modalIsActive }">
    <div class="intl-modal-wrap">
      <div class="sti">
        <i class="close-modal-btn" @click="sendCloseModal()"></i>
        <div class="country-intl-modal-top">
          <input type="text" v-model="searchValue" class="intl-search-input" placeholder="Search" />
        </div>
      </div>
      <div class="country-intl-modal-body">
        <ul class="country-intl-modal-lists" v-show="bindingData.length > 0">
          <li
            v-for="(data, i) in bindingData"
            :key="i"
            class="country-intl-modal-list"
            @click="sendCountrySelect(i)"
            :class="{
              selected: data.alpha2Code === selectedCountryInfo.alpha2Code,
            }"
          >
            <div class="country-info">
              <img class="intl-flag" :class="data.alpha2Code" />
              <span>{{ data.country.split('(')[0] }}</span>
            </div>
            <div class="country-dial">
              <span>+{{ data.dialCode }}</span>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, computed } from 'vue';
import { country } from './country-intl';

interface ISearchValueType {
  alpha2Code: string;
  country: string;
  dialCode: string;
}

const props = defineProps({
  countryIntlModalIsActive: { type: Boolean },
});

const emit = defineEmits(['sendCountrySelect', 'sendCloseModal']);
const modalIsActive = computed(() => props.countryIntlModalIsActive);
const selectedCountryInfo = ref({
  alpha2Code: 'us',
  country: 'United States',
  dialCode: '1',
});

const searchValue = ref('');

const bindingData = ref(country);

const sendCloseModal = () => {
  searchValue.value = '';
  emit('sendCloseModal', false);
};

const sendCountrySelect = (i: any) => {
  selectedCountryInfo.value = bindingData.value[i];
  emit('sendCountrySelect', selectedCountryInfo.value);
  sendCloseModal();
};

watch(searchValue, () => {
  let arr: ISearchValueType[] = [];
  for (let i = 0; i < country.length; i++) {
    arr = country.filter(
      (data) =>
        data.alpha2Code.toLowerCase().includes(searchValue.value.toLowerCase()) ||
        data.country.toLowerCase().includes(searchValue.value.toLowerCase()) ||
        data.dialCode.includes(searchValue.value),
    );
  }

  bindingData.value = arr;
});
</script>

<style lang="scss">
@import './flag';

.country-intl-modal {
  max-width: 440px;
  width: 100%;
  overflow: auto;
  overflow-x: hidden;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 500;
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  height: 0;
  margin: auto;
  transition: 0.3s;

  &::-webkit-scrollbar {
    width: 5px;
    position: fixed;
  }

  &::-webkit-scrollbar-thumb {
    background: #aaa;
    height: 0.1px;
    border-radius: 10px;
  }

  &.show {
    opacity: 1;
    visibility: visible;
    pointer-events: all;
    height: 500px;
  }

  .intl-modal-wrap {
    display: flex;
    flex-direction: column;
    background-color: #fff;

    &:hover {
      visibility: visible;
    }
  }

  .sti {
    position: sticky;
    top: 0;
  }

  .country-intl-modal-top {
    .intl-search-input {
      border: none;
      width: 100%;
      height: 52px;
      padding-top: 2px;
      padding-left: 15px;
      box-sizing: border-box;
    }
  }

  .country-intl-modal-body {
    margin-top: -10px;
    .country-intl-modal-lists {
      overflow: hidden;
      padding: 0;
    }

    .country-intl-modal-list {
      display: flex;
      justify-content: space-between;
      border-bottom: 1px solid #f3f3f3;
      padding: 5px;
      align-items: center;
      cursor: pointer;

      &:hover {
        background-color: #00d1a8;
        color: #f2f2f2;
      }

      &.selected {
        background-color: #ff4500;
        color: #f2f2f2;

        &:hover {
          background-color: #ff4500;
        }

        .country-dial {
          color: #e9e8e8;
        }
      }

      .country-info {
        font-size: 18px;
        color: #191919;

        > span {
          margin-left: 10px;
        }
      }

      .country-dial {
        color: #999595;
      }
    }
  }
}

.close-modal-btn {
  width: 52px;
  height: 52px;
  position: absolute;
  background-size: 16px;
  background-repeat: no-repeat;
  background-position: 50%;
  cursor: pointer;
  top: 0;
  right: 0;
  background-image: url('data:image/svg+xml,<svg viewBox="0 0 30 30" fill="white" xmlns="http://www.w3.org/2000/svg"><path d="M6.6129 5.2097L6.70711 5.29289L15 13.585L23.2929 5.29289C23.6834 4.90237 24.3166 4.90237 24.7071 5.29289C25.0676 5.65338 25.0953 6.22061 24.7903 6.6129L24.7071 6.70711L16.415 15L24.7071 23.2929C25.0976 23.6834 25.0976 24.3166 24.7071 24.7071C24.3466 25.0676 23.7794 25.0953 23.3871 24.7903L23.2929 24.7071L15 16.415L6.70711 24.7071C6.31658 25.0976 5.68342 25.0976 5.29289 24.7071C4.93241 24.3466 4.90468 23.7794 5.2097 23.3871L5.29289 23.2929L13.585 15L5.29289 6.70711C4.90237 6.31658 4.90237 5.68342 5.29289 5.29289C5.62334 4.96245 6.12751 4.91161 6.5114 5.14038L6.6129 5.2097Z"/></svg>');
}
</style>
