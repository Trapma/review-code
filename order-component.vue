<template v-if="filterBrands">
  <div class="wrapper-main">
    <div class="main-order">
      <div class="wrapper-content">
        <!-- хлебные крошки -->
        <bread-crumbs v-if="filterBrands" :path="breadCrumbsPath" />
        <div class="title">
          <h2>Оформление заказа</h2>
        </div>
      </div>

      <div class="main-order--body" style="margin-bottom: 40px">
        <div class="order order--wrapper">
          <div class="order--wrapper-form">
            <div class="order-block">
              <div class="textUP--wrapper">
                <span class="textUP"> МОИ ДАННЫЕ</span>
              </div>

              <div class="order-block--element-wrapper">
                <div class="form-customer">
                  <div class="form-customer--input">
                    <input
                      type="text"
                      v-maska="['+7 (###) ##-##-##', '+7 (###) ###-##-##']"
                      class="form-control style-input"
                      :class="{
                        'style-input_error': errorPhone || errorPhoneAuth,
                      }"
                      :placeholder="errorPhone ? errorPhone : '+7 ('"
                      v-model="phoneNumber"
                      @focus="(errorPhone = null), (errorPhoneAuth = null)"
                      @click="onClickInput($event)"
                      @keydown="(errorPhone = null), (errorPhoneAuth = null)"
                      :readonly="editNumberLock"
                    />
                    <transition name="animation">
                      <label
                        v-if="!errorPhone && errorPhoneAuth"
                        :class="{
                          'style-input-auth_error': errorPhoneAuth,
                        }"
                        @focus="(errorPhone = null), (errorPhoneAuth = null)"
                        @click="(errorPhone = null), (errorPhoneAuth = null)"
                        @keydown="(errorPhone = null), (errorPhoneAuth = null)"
                      >
                        <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 12 12" fill="none">
                          <path
                            fill-rule="evenodd"
                            clip-rule="evenodd"
                            d="M0.166504 6.00002C0.166504 2.77836 2.77818 0.166687 5.99984 0.166687C9.2215 0.166687 11.8332 2.77836 11.8332 6.00002C11.8332 9.22168 9.2215 11.8334 5.99984 11.8334C2.77818 11.8334 0.166504 9.22168 0.166504 6.00002ZM6.6665 3.33335C6.6665 3.70154 6.36803 4.00002 5.99984 4.00002C5.63165 4.00002 5.33317 3.70154 5.33317 3.33335C5.33317 2.96516 5.63165 2.66669 5.99984 2.66669C6.36803 2.66669 6.6665 2.96516 6.6665 3.33335ZM5.99984 5.16669C6.27598 5.16669 6.49984 5.39054 6.49984 5.66669V9.00002C6.49984 9.27616 6.27598 9.50002 5.99984 9.50002C5.72369 9.50002 5.49984 9.27616 5.49984 9.00002V5.66669C5.49984 5.39054 5.72369 5.16669 5.99984 5.16669Z"
                            fill="white"
                          />
                        </svg>
                        {{ errorPhoneAuth }}
                      </label>
                    </transition>
                    <transition name="animation">
                      <label
                        v-if="showInputDelayError"
                        :class="{
                          'style-input-auth_error': showInputDelayError,
                        }"
                      >
                        <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 12 12" fill="none">
                          <path
                            fill-rule="evenodd"
                            clip-rule="evenodd"
                            d="M0.166504 6.00002C0.166504 2.77836 2.77818 0.166687 5.99984 0.166687C9.2215 0.166687 11.8332 2.77836 11.8332 6.00002C11.8332 9.22168 9.2215 11.8334 5.99984 11.8334C2.77818 11.8334 0.166504 9.22168 0.166504 6.00002ZM6.6665 3.33335C6.6665 3.70154 6.36803 4.00002 5.99984 4.00002C5.63165 4.00002 5.33317 3.70154 5.33317 3.33335C5.33317 2.96516 5.63165 2.66669 5.99984 2.66669C6.36803 2.66669 6.6665 2.96516 6.6665 3.33335ZM5.99984 5.16669C6.27598 5.16669 6.49984 5.39054 6.49984 5.66669V9.00002C6.49984 9.27616 6.27598 9.50002 5.99984 9.50002C5.72369 9.50002 5.49984 9.27616 5.49984 9.00002V5.66669C5.49984 5.39054 5.72369 5.16669 5.99984 5.16669Z"
                            fill="white"
                          />
                        </svg>
                        для изменения номера подождите<br />еще {{ this.$store.state.auth.globalTimer }} сек.
                      </label>
                    </transition>
                  </div>
                  <div v-if="!currentCustomerPhone" class="form-customer--block-confirm">
                    <button class="btn btn-style" :class="{ disabledBtn: errorPhone }" @click="logIn()">
                      <span class="b1 text-light">Подтвердить телефон</span>
                    </button>
                  </div>

                  <div v-if="currentCustomerPhone" class="form-customer--input form-customer--input-order">
                    <input type="text" placeholder="Имя" required class="form-control style-input" v-model="customerName" />
                  </div>
                  <!--
                                    <div v-if="currentCustomerPhone" class="form-customer--edit">
                                        <button class=" b1 form-customer--btn-edit style-input btn login-text " @click="editName = true">
                                            <svg width="17" height="18" viewBox="0 0 17 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                                                <path d="M11.6066 0.5C11.7392 0.5 11.8664 0.552678 11.9602 0.646447L14.7886 3.47487C14.9838 3.67014 14.9838 3.98672 14.7886 4.18198L5.59619 13.3744C5.53337 13.4372 5.45495 13.4821 5.369 13.5046L1.54057 14.5046C1.36883 14.5494 1.18617 14.4999 1.06066 14.3744C0.93514 14.2489 0.885581 14.0662 0.930441 13.8945L1.93044 10.066C1.95289 9.98007 1.99784 9.90165 2.06066 9.83883L11.253 0.646447C11.3468 0.552678 11.474 0.5 11.6066 0.5Z" fill="#7A2A9A"/>
                                                <path d="M1 16.25C0.585786 16.25 0.25 16.5858 0.25 17C0.25 17.4142 0.585786 17.75 1 17.75H16C16.4142 17.75 16.75 17.4142 16.75 17C16.75 16.5858 16.4142 16.25 16 16.25H1Z" fill="#7A2A9A"/>
                                            </svg>
                                            редактировать
                                        </button>
                                    </div>
                                    -->
                </div>
                <div v-if="!currentCustomerPhone" class="form-customer--another">
                  <div class="form-customer--confirm">
                    <span class="b2 b2-normal b2-opacity">Указывая номер телефона вы принимаете условия&nbsp;</span>
                    <a href="https://darall.pro/concent-individual.pdf" target="_blank" class="b2 b2-normal text">пользовательского соглашения</a>
                    <!-- добавила ссылку-->
                  </div>
                </div>
              </div>
            </div>

            <div class="order-block--radio">
              <div class="form-check form-check-inline mr-3">
                <!-- style="position:absolute;" -->
                <input
                  @change="clearError"
                  class="form-check-input custom-radio"
                  v-model="this.$store.state.order.deliveryType"
                  id="radio1"
                  type="radio"
                  checked="checked"
                  name="inlineRadioType"
                  value="1"
                />
                <label class="form-check-label" for="radio1">Доставка</label>
              </div>
              <div class="form-check form-check-inline mr-3">
                <input
                  @change="clearError"
                  :disabled="toManyBrands"
                  class="form-check-input custom-radio"
                  v-model="this.$store.state.order.deliveryType"
                  id="radio2"
                  type="radio"
                  name="inlineRadioType"
                  value="2"
                />
                <label class="form-check-label" for="radio2">Самовывоз</label>
              </div>
              <div class="form-check form-check-inline mr-3">
                <input
                  @change="clearError"
                  class="form-check-input custom-radio"
                  v-model="this.$store.state.order.deliveryType"
                  id="radio3"
                  type="radio"
                  name="inlineRadioType"
                  value="3"
                />
                <label class="form-check-label" for="radio3">Доставка другому</label>
              </div>
            </div>

            <!--ТИП ДОСТАВКИ -->
            <template v-if="Number(deliveryType) === 3">
              <div class="order-block">
                <div class="form-customer">
                  <div class="form-customer--name">
                    <input
                      type="text"
                      name="name"
                      required
                      class="form-control style-input"
                      :class="{
                        'style-input_error': errorAnotherName,
                      }"
                      :placeholder="errorAnotherName ? errorAnotherName : 'Получатель'"
                      v-model="anotherCustomerName"
                      @focus="errorAnotherName = null"
                      @click="errorAnotherName = null"
                      @keydown="errorAnotherName = null"
                    />
                  </div>
                  <div class="form-customer--input">
                    <input
                      type="text"
                      v-maska="['+7 (###) ##-##-##', '+7 (###) ###-##-##']"
                      class="form-control style-input"
                      :class="{
                        'style-input_error': errorAnotherPhone,
                      }"
                      :placeholder="errorAnotherPhone ? errorAnotherPhone : '+7 ('"
                      v-model="anotherCustomerPhone"
                      @focus="errorAnotherPhone = null"
                      @click="errorAnotherPhone = null"
                      @keydown="errorAnotherPhone = null"
                    />
                  </div>
                </div>
              </div>
            </template>
            <!--МОИ АДРЕСА && АДРЕС ДОСТАВКИ-->
            <template v-if="Number(deliveryType) !== 2">
              <div style="position: relative" class="order-block order--min-height">
                <div>
                  <div class="textUP--wrapper">
                    <span class="textUP mt-3">{{ addressInfo }}</span>
                  </div>
                  <div style="position: relative" v-if="addresses.length" class="order-block--element-wrapper address-item--wrapper">
                    <!--v-if="loadingAddress" -->
                    <div v-if="loadingAddress" class="loader--position">
                      <div class="loader">
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                        <div class="loader__stick"></div>
                      </div>
                    </div>

                    <div
                      class="address-item align-items-center"
                      :class="{
                        selectAddress: selectedAddress ? selectedAddress.id === address.id : false,
                      }"
                      v-for="address in addresses"
                      :key="address.id"
                      @click="selectAddress(address)"
                    >
                      <div style="width: 24px">
                        <transition name="animationReverse">
                          <div class="address-item--check" v-if="selectedAddress ? selectedAddress.id === address.id : false">
                            <svg xmlns="http://www.w3.org/2000/svg" width="19" height="15" viewBox="0 0 19 15" fill="none">
                              <path d="M6.49996 10.4834L2.38371 6.36719L0.616211 8.13469L6.49996 14.0184L18.6337 1.88469L16.8662 0.117188L6.49996 10.4834Z" fill="#21C777" />
                            </svg>
                          </div>
                        </transition>
                      </div>
                      <div class="address-item--text">
                        <div class="align-items-center justify-content-between">
                          <span class="b1" style="text-transform: capitalize"> {{ address.full_addr }}</span>
                        </div>
                        <!-- v-if="!address.actual" -->
                        <button @click.stop="deleteAddress(address)" title="удалить" class="address-item--delete-icon">
                          <svg width="14" height="18" viewBox="0 0 14 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path
                              d="M4.70768 1.5C4.70768 1.15482 4.9875 0.875 5.33268 0.875H8.66602C9.01119 0.875 9.29102 1.15482 9.29102 1.5V2.125H12.8327C13.1779 2.125 13.4577 2.40482 13.4577 2.75C13.4577 3.09518 13.1779 3.375 12.8327 3.375H1.16602C0.820838 3.375 0.541016 3.09518 0.541016 2.75C0.541016 2.40482 0.820838 2.125 1.16602 2.125H4.70768V1.5Z"
                              fill="#0C043F"
                            />
                            <path
                              fill-rule="evenodd"
                              clip-rule="evenodd"
                              d="M2.19926 5.62065C2.22271 5.40964 2.40107 5.25 2.61338 5.25H11.3853C11.5976 5.25 11.776 5.40964 11.7994 5.62065L11.9662 7.12178C12.2669 9.82759 12.2669 12.5584 11.9662 15.2642L11.9498 15.412C11.8403 16.3975 11.0765 17.1833 10.0946 17.3207C8.0411 17.6082 5.95759 17.6082 3.90413 17.3207C2.92216 17.1833 2.15839 16.3975 2.04889 15.412L2.03247 15.2642C1.73183 12.5584 1.73183 9.82759 2.03247 7.12177L2.19926 5.62065ZM5.95768 8.49999C5.95768 8.15482 5.67786 7.87499 5.33268 7.87499C4.9875 7.87499 4.70768 8.15482 4.70768 8.49999L4.70768 14.3333C4.70768 14.6785 4.9875 14.9583 5.33268 14.9583C5.67786 14.9583 5.95768 14.6785 5.95768 14.3333L5.95768 8.49999ZM9.29102 8.49999C9.29102 8.15482 9.0112 7.87499 8.66602 7.87499C8.32084 7.87499 8.04102 8.15482 8.04102 8.49999V14.3333C8.04102 14.6785 8.32084 14.9583 8.66602 14.9583C9.01119 14.9583 9.29102 14.6785 9.29102 14.3333V8.49999Z"
                              fill="#0C043F"
                            />
                          </svg>
                        </button>
                      </div>
                    </div>
                  </div>
                  <div v-if="hasMyAddress && hiddenAddBtn === true" class="order-block--element-wrapper">
                    <div class="textUP--wrapper">
                      <button @click="addNewAddress" class="btn-style btn"><span class="b1 text-light">Добавить новый адрес</span></button>
                    </div>
                  </div>

                  <div v-show="showAddAddress" class="order-block--element-wrapper">
                    <div class="form-customer">
                      <div class="form-customer--input form-customer--input-street">
                        <!--:address="addressState.name"-->
                        <input
                          class="style-input form-control order-address-street--input"
                          :value="this.$store.state.address.name"
                          width="100%"
                          @click="openMap"
                          type="text"
                          placeholder="Улица, дом"
                          readonly
                        />
                      </div>
                      <div class="form-customer form-customer--options form-customer--options-wrapper">
                        <div class="form-customer--input form-customer--input-options">
                          <input
                            type="text"
                            name="entrance"
                            required
                            v-maska="['##', '##', '##']"
                            class="form-control style-input"
                            :class="{
                              'style-input_error': errorEntrance,
                            }"
                            :placeholder="errorEntrance ? errorEntrance : 'Подъезд'"
                            v-model="entrance"
                            @focus="errorEntrance = null"
                            @click="errorEntrance = null"
                            @keydown="errorEntrance = null"
                          />
                        </div>
                        <div class="form-customer--input form-customer--input-options form-customer--last">
                          <input
                            type="text"
                            name="apartment"
                            required
                            v-maska="['###', '###', '###']"
                            class="form-control style-input"
                            :class="{
                              'style-input_error': errorApartment,
                            }"
                            :placeholder="errorApartment ? errorApartment : 'Кв / офис'"
                            v-model="apartment"
                            @focus="errorApartment = null"
                            @click="errorApartment = null"
                            @keydown="errorApartment = null"
                          />
                        </div>
                      </div>
                    </div>
                    <div class="address-wrapper">
                      <div class="form-customer form-customer--options form-customer--options-wrapper">
                        <div class="form-customer--input form-customer--input-options">
                          <input
                            type="text"
                            name="floor"
                            required
                            v-maska="['###', '###', '###']"
                            class="form-control style-input"
                            :class="{
                              'style-input_error': errorFloor,
                            }"
                            :placeholder="errorFloor ? errorFloor : 'Этаж'"
                            v-model="floor"
                            @focus="errorFloor = null"
                            @click="errorFloor = null"
                            @keydown="errorFloor = null"
                          />
                        </div>
                        <div class="form-customer--input form-customer--input-comment form-customer--last">
                          <input type="text" name="comment" class="form-control style-input" placeholder="Комментарий" v-model="comment" />
                        </div>
                      </div>
                    </div>
                  </div>
                  <div v-show="!showAddAddress" class="form-customer--options form-customer--options-wrapper">
                    <div class="form-customer order-block--element-wrapper">
                      <div style="width: 100%" class="form-customer--input-comment form-customer--input-comment">
                        <input type="text" name="comment" class="form-control style-input" placeholder="Комментарий" v-model="comment" />
                      </div>
                    </div>
                  </div>
                  <div class="row" v-if="showForm === true && hasMyAddress === true">
                    <div class="col-12 mt-3">
                      <div class="row">
                        <div class="pl-0 col-12">
                          <button @click="cancelAddress" class="btn-style btn btn-newAddress-cancel mx-3">
                            <span>Отмена</span>
                          </button>
                          <button @click="saveNewAddress" class="btn-style btn text-light btn-newAddress">
                            <span>Сохранить</span>
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!--
                                <div v-if="showError" class="row">
                                    <div class="col-12">
                                        <span style="color: red;"> {{noEmptyAddress}}</span>
                                    </div>
                                </div>
                                -->
              </div>
            </template>

            <div class="order-block">
              <div class="">
                <span class="textUP">{{ deliveryTimeInfo }}</span>
              </div>

              <div class="">
                <!--style="position:absolute;" v-if="!(selectDay === preOrderDays[0].date && selectTime==='Выходной')" !(selectDay === preOrderDays[0].date && selectTime==='Выходной')-->
                <div class="form-check form-check-inline mr-3">
                  <input
                    @change="clearError"
                    class="form-check-input custom-radio"
                    v-model="deliveryMode"
                    id="radio4"
                    type="radio"
                    name="inlineRadioMode"
                    :disabled="restaurantClose || pointClose"
                    :checked="deliveryMode == 1"
                    value="1"
                  />
                  <label class="form-check-label" for="radio4">Ближайшее</label>
                </div>
                <!-- selectDay === preOrderDays[0].date && selectTime==='Выходной' -->
                <div class="form-check form-check-inline mr-3">
                  <input
                    @change="clearError"
                    class="form-check-input custom-radio"
                    v-model="deliveryMode"
                    id="radio5"
                    type="radio"
                    name="inlineRadioMode"
                    :checked="deliveryMode == 2"
                    value="2"
                  />
                  <label class="form-check-label" for="radio5">Запланировать</label>
                </div>
              </div>
            </div>

            <template v-if="Number(deliveryMode) === 2">
              <!--:selected="selectDay"-->
              <div class="form-customer form-customer--options-wrapper">
                <div class="form-customer--input form-customer--input-select form-customer--input-select-date">
                  <select
                    v-model="selectDay"
                    class="custom-select style-input"
                    :class="{
                      'style-select_error': errorDay,
                    }"
                    @focus="errorDay = null"
                    @click="errorDay = null"
                    @keydown="errorDay = null"
                  >
                    <option v-for="day in preOrderDays" :key="day" :value="day.date">{{ day.title }}</option>
                  </select>
                  <transition name="animation">
                    <div
                      v-if="errorPreOrderTime"
                      :class="{
                        'style-input-auth_error': errorPreOrderTime,
                      }"
                      @focus="(errorPreOrderTime = null), (errorPreOrderTime = null)"
                      @click="(errorPreOrderTime = null), (errorPreOrderTime = null)"
                      @keydown="(errorPreOrderTime = null), (errorPreOrderTime = null)"
                    >
                      <svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 12 12" fill="none">
                        <path
                          fill-rule="evenodd"
                          clip-rule="evenodd"
                          d="M0.166504 6.00002C0.166504 2.77836 2.77818 0.166687 5.99984 0.166687C9.2215 0.166687 11.8332 2.77836 11.8332 6.00002C11.8332 9.22168 9.2215 11.8334 5.99984 11.8334C2.77818 11.8334 0.166504 9.22168 0.166504 6.00002ZM6.6665 3.33335C6.6665 3.70154 6.36803 4.00002 5.99984 4.00002C5.63165 4.00002 5.33317 3.70154 5.33317 3.33335C5.33317 2.96516 5.63165 2.66669 5.99984 2.66669C6.36803 2.66669 6.6665 2.96516 6.6665 3.33335ZM5.99984 5.16669C6.27598 5.16669 6.49984 5.39054 6.49984 5.66669V9.00002C6.49984 9.27616 6.27598 9.50002 5.99984 9.50002C5.72369 9.50002 5.49984 9.27616 5.49984 9.00002V5.66669C5.49984 5.39054 5.72369 5.16669 5.99984 5.16669Z"
                          fill="white"
                        />
                      </svg>
                      {{ errorPreOrderTime }}
                    </div>
                  </transition>
                </div>

                <div style="position: relative" class="form-customer--input form-customer--input-select form-customer--input-select-time">
                  <div
                    v-if="!preOrderTime"
                    class="loader--position"
                    :class="{
                      'style-input_error': preOrderTimeError,
                    }"
                  >
                    <div class="loader">
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                    </div>
                  </div>
                  <select
                    v-model="selectTime"
                    class="custom-select style-input"
                    :disabled="selectTime === 'Выходной'"
                    @focus="errorTime = null"
                    @click="errorTime = null"
                    @keydown="errorTime = null"
                  >
                    <option :selected="selectTime" v-for="time in preOrderTime" :key="time" :value="time">{{ time }}</option>
                  </select>
                </div>
              </div>
            </template>

            <template class="form-block" v-if="selfDelivery">
              <div class="textUP--wrapper">
                <span class="textUP">АДРЕС ВЫДАЧИ</span>
              </div>

              <div class="form-customer form-customer--options-wrapper">
                <div style="position: relative" class="form-customer--input form-customer--input-select form-customer--input-select-self">
                  <div v-if="!brandPoints.length" class="loader--position">
                    <div class="loader">
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                      <div class="loader__stick"></div>
                    </div>
                  </div>
                  <select
                    id="inputGroupSelect01"
                    v-model="pointSelfDelivery"
                    class="custom-select style-input"
                    :class="{
                      'style-select_error': errorPointSelfDelivery,
                    }"
                    @focus="errorPointSelfDelivery = null"
                    @click="errorPointSelfDelivery = null"
                    @keydown="errorPointSelfDelivery = null"
                  >
                    <!--
                                        :selected="nearestPoint? point === nearestPoint : point" -->

                    <option :selected="pointSelfDelivery" v-for="point in brandPoints" :key="point.id" :value="point">{{ point.address }}</option>
                  </select>
                </div>
              </div>
            </template>
            <div class="cart-btn--footer-pay-wrapper">
              <div class="cart-btn--footer-pay">
                <!-- :disabled="errorTotalAmount" -->
                <button
                  v-if="addressState.name || !viewBtnChooseLocation"
                  @click="checkOrder()"
                  :disabled="deliveryPriceCondition && minPriceExist"
                  class="cart-btn--footer-pay-btn"
                >
                  <span style="float: left">{{ deliveryPriceCondition && minPriceExist ? "Добавить еще на" : "Оформить " + (deliveryMode == 2 ? "предзаказ" : "заказ") }}</span>
                  <span style="float: right"
                    >{{ deliveryPriceCondition && minPriceExist ? getMinPriceDelivery : deliveryPriceCondition && !selfDeliveryTypeExist ? totalAmount : getAmountCart }} ₽</span
                  >
                </button>
                <!-- button v-if="!addressState.name && viewBtnChooseLocation" @click="showDeliveryModal()" class="cart-btn--footer-pay-btn"><span>Указать свой адрес</span> -->
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-if="addToOrderShow.length" class="wrapper-content row" style="margin-bottom: 50px">
        <h4 class="col-12">Рекомендуем добавить к заказу сейчас:</h4>

        <div class="shell-card-dish" v-for="item in addToOrderShow" :key="item.id" style="margin-bottom: 10px; z-index: 0">
          <div class="card card-brand img-brand-wrapper" style="width: 100%" v-if="item.status && !item.delete">
            <div class="style-img-dish">
              <div class="card-img-top-hover">
                <div class="card-hover-text">
                  <!--<button :disabled="orderState.loadingStatus === 'loading'" class="btn btn-buy-dish" @click="clickAddCart(item)">Купить</button>-->
                  <button class="btn btn-more-details-dish" @click="clickMoreDetails(item)">
                    Подробнее
                    <svg width="30" height="30" viewBox="0 0 30 30" fill="none" xmlns="http://www.w3.org/2000/svg">
                      <path
                        d="M9.08873 9.02812H15.3619C15.4804 9.02812 15.5774 8.93115 15.5774 8.81263V7.56339C15.5775 7.25228 15.3252 7 15.0141 7H9.43658C9.12547 7 8.87324 7.25228 8.87324 7.56339V8.81263C8.87324 8.93121 8.97021 9.02812 9.08873 9.02812Z"
                        fill="#7A2A9A"
                      />
                      <path
                        d="M8.2898 10.8873H16.1608C16.2794 10.8873 16.333 10.8006 16.2799 10.6945L15.9375 10.0097C15.8845 9.90365 15.7442 9.81692 15.6257 9.81692H8.82502C8.7065 9.81692 8.56617 9.90365 8.51316 10.0097L8.17069 10.6945C8.11768 10.8006 8.17129 10.8873 8.2898 10.8873Z"
                        fill="#7A2A9A"
                      />
                      <path
                        d="M16.4788 19.6479C16.4788 19.0104 16.6623 18.1563 16.9862 17.3322C16.2594 17.2942 15.1668 17.517 14.2907 17.8864C13.278 18.3135 12.6729 18.8576 12.6307 19.3792C12.5883 19.9009 13.0978 20.5353 14.0286 21.12C14.8625 21.6437 15.9513 22.0506 16.6766 22.1094C16.9728 22.1335 17.2653 22.103 17.5436 22.0211C16.8908 21.4378 16.4788 20.5902 16.4788 19.6479Z"
                        fill="#7A2A9A"
                      />
                      <path
                        d="M21.2841 16.9285C20.7765 15.9535 20.1853 15.3943 19.662 15.3943C19.1386 15.3943 18.5474 15.9535 18.0398 16.9285C17.5852 17.8019 17.2676 18.9202 17.2676 19.6479C17.2676 20.9681 18.3417 22.0422 19.662 22.0422C20.9822 22.0422 22.0563 20.9681 22.0563 19.6479C22.0563 18.9202 21.7388 17.8019 21.2841 16.9285Z"
                        fill="#7A2A9A"
                      />
                      <path
                        d="M11.8444 19.3155C11.9123 18.4785 12.6722 17.7129 13.9843 17.1596C14.7847 16.822 15.7266 16.5997 16.5071 16.5501V11.8915C16.5071 11.773 16.4101 11.676 16.2916 11.676H8.15916C8.04065 11.676 7.94368 11.773 7.94368 11.8915V22.4366C7.94368 22.7477 8.19596 23 8.50707 23H15.9437C16.0863 23 16.2163 22.9467 16.3155 22.8592C14.5501 22.578 11.7041 21.0474 11.8444 19.3155Z"
                        fill="#7A2A9A"
                      />
                      <path
                        d="M29.2857 5C29.2857 4.60551 28.9659 4.28571 28.5714 4.28571H25V0.714285C25 0.319796 24.6802 0 24.2857 0C23.8912 0 23.5714 0.319797 23.5714 0.714286V4.28571H20C19.6055 4.28571 19.2857 4.60551 19.2857 5C19.2857 5.39449 19.6055 5.71429 20 5.71429H23.5714V9.28572C23.5714 9.6802 23.8912 10 24.2857 10C24.6802 10 25 9.6802 25 9.28571V5.71429H28.5714C28.9659 5.71429 29.2857 5.39449 29.2857 5Z"
                        fill="#0C043F"
                      />
                    </svg>
                  </button>
                </div>
              </div>
              <img :src="item.picture" class="card-img-top" style="border-radius: 30px 30px 0px 0px" />

              <button v-if="this.customer" class="btn btn-favorite" @click="clickFavorite(item.id)">
                <svg v-if="checkFavorite(item.id)" width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path
                    class="back-favorite"
                    d="M0 18C0 8.05887 8.05887 0 18 0V0C27.9411 0 36 8.05887 36 18V18C36 27.9411 27.9411 36 18 36V36C8.05887 36 0 27.9411 0 18V18Z"
                    fill="#EAEBEE"
                  />
                  <path
                    d="M14.6998 11.8125C12.1507 11.8125 9.979 13.7185 9.979 16.194C9.979 17.9017 10.7787 19.3394 11.8385 20.5304C12.8946 21.7173 14.2572 22.7135 15.4891 23.5473L17.6145 24.986C17.8472 25.1436 18.1525 25.1436 18.3852 24.986L20.5106 23.5473C21.7425 22.7135 23.1051 21.7173 24.1612 20.5304C25.221 19.3394 26.0207 17.9017 26.0207 16.194C26.0207 13.7185 23.849 11.8125 21.2998 11.8125C19.9858 11.8125 18.8296 12.4286 17.9998 13.2258C17.1701 12.4286 16.0139 11.8125 14.6998 11.8125Z"
                    fill="#F52C2C"
                  />
                </svg>
                <svg v-else width="36" height="36" viewBox="0 0 36 36" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path class="back-favorite" d="M0 18C0 8.05888 8.05888 0 18 0C27.9411 0 36 8.05888 36 18C36 27.9411 27.9411 36 18 36C8.05888 36 0 27.9411 0 18Z" fill="#EAEBEE" />
                  <g opacity="0.5">
                    <path
                      d="M14.7003 11.8125C12.1512 11.8125 9.97949 13.7185 9.97949 16.194C9.97949 17.9017 10.7791 19.3394 11.8389 20.5304C12.8951 21.7173 14.2577 22.7135 15.4896 23.5473L17.6149 24.986C17.8477 25.1436 18.1529 25.1436 18.3857 24.986L20.5111 23.5473C21.7429 22.7135 23.1056 21.7173 24.1617 20.5304C25.2215 19.3394 26.0212 17.9017 26.0212 16.194C26.0212 13.7185 23.8494 11.8125 21.3003 11.8125C19.9863 11.8125 18.8301 12.4286 18.0003 13.2258C17.1706 12.4286 16.0143 11.8125 14.7003 11.8125Z"
                      fill="#0C043F"
                    />
                  </g>
                </svg>
              </button>
            </div>
            <div class="card-body">
              <div class="card-title title-dish row">
                <div class="col reduction-word" data-bs-toggle="tooltip" :title="item.name" data-bs-placement="bottom">
                  <!-- item.name.substr(0, 17) + (item.name.length > 17 ?"..":'') -->
                  {{ item.name }}
                </div>
                <span
                  class="col"
                  style="white-space: nowrap"
                  data-bs-toggle="tooltip"
                  :title="item.weight + getPortionType(item.settings.portion_type)"
                  data-bs-placement="bottom"
                  >{{ item.weight + getPortionType(item.settings.portion_type) }}</span
                >
              </div>
            </div>
            <div class="card-footer footer-dish row">
              <div class="col-3 price" style="padding: 0px" data-bs-toggle="tooltip" :title="item.price + ' ₽'" data-bs-placement="bottom">{{ item.price }} ₽</div>

              <div class="col-9" style="text-align: end; padding: 0">
                <!--<span v-if="item.count" class="count-dish-order" style="margin-right: 6px;">{{item.count + ' x'}}</span>-->
                <div v-if="!item.count" class="div-more-details view-mobile-more-details">
                  <span class="more-details" style="margin-right: 2px" @click="clickMoreDetails(item)"> Подробнее </span>
                  <button v-if="item.adds_dish.length" class="btn-adds-more" style="margin-right: 2px" @click="clickMoreDetails(item)">
                    <svg width="16" height="16" viewBox="0 0 16 16" fill="none" xmlns="http://www.w3.org/2000/svg">
                      <path
                        d="M2.08902 2.02812H8.36221C8.48073 2.02812 8.5777 1.93115 8.5777 1.81263V0.563394C8.57775 0.252283 8.32547 0 8.01441 0H2.43688C2.12576 0 1.87354 0.252283 1.87354 0.563394V1.81263C1.87354 1.93121 1.9705 2.02812 2.08902 2.02812Z"
                        fill="white"
                      />
                      <path
                        d="M1.28996 3.88728H9.16099C9.27951 3.88728 9.33312 3.80054 9.2801 3.69452L8.93769 3.00965C8.88468 2.90363 8.74435 2.81689 8.62583 2.81689H1.82518C1.70666 2.81689 1.56633 2.90363 1.51332 3.00965L1.17085 3.69452C1.11784 3.80054 1.17144 3.88728 1.28996 3.88728Z"
                        fill="white"
                      />
                      <path
                        d="M9.47905 12.648C9.47905 12.0106 9.66254 11.1564 9.98636 10.332ё13C9.25958 10.2944 8.16705 10.5171 7.29093 10.8865C6.27819 11.3136 5.6731 11.8578 5.63086 12.3793C5.58852 12.901 6.09804 13.5355 7.02879 14.1202C7.86267 14.6439 8.95154 15.0507 9.67681 15.1095C9.97306 15.1336 10.2655 15.1031 10.5438 15.0212C9.89101 14.4379 9.47905 13.5903 9.47905 12.648Z"
                        fill="white"
                      />
                      <path
                        d="M14.284 9.92864C13.7765 8.95367 13.1852 8.39453 12.6619 8.39453C12.1386 8.39453 11.5473 8.95367 11.0398 9.92864C10.5852 10.8021 10.2676 11.9203 10.2676 12.6481C10.2676 13.9683 11.3417 15.0424 12.6619 15.0424C13.9822 15.0424 15.0563 13.9682 15.0563 12.6481C15.0563 11.9203 14.7387 10.8021 14.284 9.92864Z"
                        fill="white"
                      />
                      <path
                        d="M4.84461 12.3158C4.91249 11.4787 5.67236 10.7131 6.98446 10.1598C7.78484 9.82223 8.72679 9.59996 9.50724 9.55029V4.89176C9.50724 4.77324 9.41027 4.67627 9.29175 4.67627H1.15934C1.04082 4.67627 0.943848 4.77324 0.943848 4.89176V15.4368C0.943848 15.7479 1.19613 16.0002 1.50724 16.0002H8.94384C9.0865 16.0002 9.21649 15.9469 9.31567 15.8594C7.55023 15.5782 4.70422 14.0476 4.84461 12.3158Z"
                        fill="white"
                      />
                    </svg>
                  </button>
                </div>
                <button class="btn-add-cart adds-dish-and-no-count" @click="plusCountAddToOrder(item)">
                  <span>Купить</span>
                  <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M14 6H8V0H6V6H0V8H6V14H8V8H14V6Z" fill="#0C043F" />
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div>
      <cart @checkOrder="checkOrder()" @addsMore="addsDishMore" :preOrderTime="preOrderTime"></cart>
    </div>
  </div>
</template>

<script>
// import SavePaymentCardsModal from './pay/SavePaymentCardModal.vue'
// import AddPaymentCardModal from './pay/AddPaymentCardModal.vue'
// import ConfirmPayModal from './pay/ConfirmPayModal.vue'
import BreadCrumbs from "./additional/BreadCrumbs.vue";
import Cart from "./Cart.vue";
import config from "../config";
import $ from "jquery";
import { mapState, mapGetters, mapActions } from "vuex";
// import { useHead } from '@vueuse/head'
// import { computed } from 'vue'

export default {
  name: "Order",
  // AddPaymentCardModal, ConfirmPayModal, SavePaymentCardsModal,
  components: { Cart, BreadCrumbs },
  data() {
    const SHOW_MODAL = {
      hide: -1,
      saveCardsModal: 0,
      addCardModal: 1,
      confirmPayModal: 2,
      widget: 3,
    };
    return {
      SHOW_MODAL,
      // время сейчас (обновляется каждую минуту)
      timeNow: "",
      // часы с интервалом в минуту
      startClock: null,
      content: "",
      valid: false,
      showModal: SHOW_MODAL.hide,
      currency: "RUB",
      urlRedirect: "",
      cardId: null,
      // редактирование имени
      editName: false,

      showForm: false,
      showSelectOptions: false,
      editStreet: false,
      checkUserAgreement: true,
      hiddenAddBtn: true,

      // mainHeight: 550,
      // номер пользователя
      phoneNumber: null,
      // имя пользователя
      customerName: "",
      // номер другого пользователя, для "доставка другому"
      anotherCustomerPhone: "",
      // имя другого пользователя, для "доставка другому"
      anotherCustomerName: "",

      // street: null,
      // streetField: null,
      entrance: null,
      apartment: null,
      floor: null,
      comment: "",
      // выбранный адрес
      selectedAddress: null,
      //сохраненные адреса если есть
      // saveAddress: [],

      //вид доставки 1) доставка 2) самовывоз 3) заказ другому (возможно нужно будет переименовывать)
      // deliveryType: 1,
      // тип доставки 1)"🔥 Ближайший" 2) "⏰ Предзаказ"
      deliveryMode: null,
      deliveryRadioButtonDisabled: false,

      // выбранный день предзаказа
      selectDay: null,
      // выбранное время предзаказа
      selectTime: null,
      dayOff: false,

      // выбраный пункт для самовывоза
      pointSelfDelivery: null,

      filterBrandsWithPoints: null,

      errorPhoneAuth: null,
      errorPhone: null,
      errorAnotherPhone: null,
      errorAnotherName: null,
      errorAddress: null,
      errorEntrance: null,
      errorFloor: null,
      errorApartment: null,
      errorTime: null,
      errorDay: null,
      errorPointSelfDelivery: null,
      errorPreOrderTime: null,

      showInputDelayError: false,
      //isUpgradeAddsToOrder: false
    };
  },
  updated() {
    //срабатывает при изменении данных в dom дереве
  },
  beforeUnmount() {
    // prevent memory leak
    clearInterval(this.startClock);
  },

  async mounted() {
    // this.deliveryMode = this.selectDay === this.preOrderDays[0].date && this.selectTime==='Выходной' ? 2 : 1
    // this.deliveryMode = this.todayDayOff ? 2 : 1
    this.deliveryMode = this.restaurantClose ? 2 : this.$store.state.order.deliveryMode ? this.$store.state.order.deliveryMode : 1;

    // запускаем часы с интервалом в минуту, для пересчета массива ближайших заказов
    this.startClock = setInterval(() => {
      // ! Отображать если немного времени до закрытия осталось
      this.timeNow = new Date();
      console.log("date now", this.timeNow);
      this.$store.dispatch("getStatusBrand", this.filterBrands[0].id).then((res) => {
        console.log("check status brand", res);
        if (!res.status) {
          // перебросить на главную и очистить корзину
          this.$router.push({ name: "homeBrand", params: { brand: "food" } });
          this.$store.commit("order/CLEAN_CART");
          this.$store.dispatch("order/updateCartData");
        }
      });
    }, 60000);

    // TODO сделать лонг пулинг для отслеживания статуса бренда (попробовать в будущем)

    this.$store.dispatch("order/upgradeAddsToOrder");

    this.scrollStart();
    // проверяет не истекло ли время таймера
    if (0 < this.$store.getters["auth/getTimer"] && this.$store.getters["auth/getTimer"] < 60) {
      // ищет телефон на который пришел код
      if (window.localStorage.getItem("numberWithSMS")) {
        this.phoneNumber = window.localStorage.getItem("numberWithSMS");
      }
      // запускает таймер
      this.$store.dispatch("auth/runGlobalTimer");
    }

    if (this.filterBrands) {
      this.getPointsByBrandsId();
      // const id = this.getOrderBrandId[0]
      // const brand = this.$store.state.brands.filter(item => item.id === id)
      // const brandName = brand?.name
      // const brandNameTranslit = brand?.translit_name
    }
    console.log("test this.preOrderDays", this.preOrderDays);
    if (this.preOrderDays) {
      this.selectDay = this.preOrderDays[0].date;
    }

    this.selectTime = this.preOrderTime[0];
    this.pointSelfDelivery = this.brandPoints[0];

    // this.scrollBrands()
    // this.mainHeight = document.querySelector(".main-order").getBoundingClientRect().height

    if (!this.brands.length && this.getOrderBrandId[0]) {
      // this.getBrandById(this.getOrderBrandId[0])
      // this.getBrands()
    }
    this.content = "Заказы";

    //если пользователь зарегестрирован
    if (this.customer) {
      console.log("customer mounted", this.customer);
      if (this.getActualAddress) {
        // в процессе заказа, был выбран другой адрес
        if (this.addressState.name !== this.getActualAddress.street_id) {
          this.addNewAddress();
        } else {
          this.selectedAddress = this.getActualAddress;
        }
      }

      if (!this.addressState.addresses.length) {
        this.$store.dispatch("address/getAddress", this.customer.id);
      }

      this.phoneNumber = this.customer.number;
      this.customerName = this.customer.name;

      // const res = await this.getCustomerName(this.phoneNumber)
      // if(res?.success && res?.customer_name) {
      //     this.customerName = res.customer_name
      // }

      // this.saveAddress = this.addressState.addresses
    }

    if (this.addressState.name && this.addressState.coord && this.addressState.coord.length) {
      this.street = this.addressState.name;
      //    const streetName = this.addressState.name.split(',')

      //    if(streetName.length > 2) {
      //        this.street = streetName[1] + streetName[2]
      //    }
      //    if(streetName.length === 2) {
      //        this.street = this.addressState.name
      //    }
      //    if(streetName.length === 1) {
      //        this.street = this.addressState.name
      //    }
    }

    // const aside = document.querySelector('#app > div > div.container-fluid.p-0 > div > div.d-flex > div.d-none.d-md-block.col-md-4.trash-position')
    // const footer = document.querySelector('#app > footer > div')
    // const trash = document.querySelector('#app > div > div.container-fluid.p-0 > div > div.d-flex > div.d-none.d-md-block.col-md-4.trash-position > div > div.cart.trash')

    // window.addEventListener('scroll', () => {
    //     console.log(aside.getBoundingClientRect().top)
    //     console.log(footer.getBoundingClientRect().top)
    //     console.log(trash.getBoundingClientRect().top)
    // })

    /*useHead({
                title: computed(() =>  'Оформление заказа - Market Darall'),
                meta: [
                    {
                    name: `description`,
                    content: computed(() =>  'Оформление заказа - Market Darall'),
                    },
                ],
            })*/
  },
  watch: {
    customer: function (val) {
      console.log("customer");
      this.phoneNumber = val.number;
      this.customerName = val.name;
      if (!this.addressState.addresses.length) {
        this.$store.dispatch("address/getAddress", val.id);
      }
    },
    brandPoints: function (val) {
      this.pointSelfDelivery = val[0];
    },
    getActualAddress: function (val) {
      this.selectedAddress = val;
    },
    // getOrderBrandId: function(val){
    //      console.log('test getOrderBrandId watch = ', val)
    //     this.getBrandById(val[0])
    // },
    preOrderTime: function (val) {
      this.selectTime = val[0];
      console.log(val);
      if (val[0] === "Выходной" || val[0] === "Закрыто") {
        this.deliveryMode = 2;
        this.closeModal();
      } else if (val[0] === "Ближайший") {
        this.deliveryMode = 1; // 2
      } else {
        this.errorPreOrderTime = null;
      }
    },
    // workDayTime: function(val){
    //     if(this.deliveryMode === 1){
    //         this.deliveryMode = !val ? 2 : 1
    //     }
    // },
    // todayDayOff: function(val){
    //     this.deliveryMode = val ? 2 : 1
    // },
    restaurantClose: function (val) {
      this.deliveryMode = val ? 2 : 1;
    },

    checkDeliveryAddressModule: function () {
      return this.addNewAddress();
    },
    // addressState: function (val) {
    //     console.log('address ', val)
    //     console.log('this.getActualAddress = ', this.getActualAddress)

    //     this.saveAddress = val.addresses
    //     this.selectedAddress = this.getActualAddress
    //     this.street = val.name

    // },
    // selectDay: function(val) {
    //     console.log(val)
    //     const date = new Date(val)
    //     console.log(date)
    // },
    // selectTime: function(val) {
    //     console.log(val)
    // },
    preOrderDate: function (val) {
      console.log(val);
    },
    filterBrands: function (val) {
      if (val.length) {
        return this.getPointsByBrandsId();
      }
    },
    preOrderDays: function (val) {
      this.selectDay = val[0].date;
    },
    selectTime: function (val) {
      if (val[0] === "Закрыто") {
        this.$store.commit("order/PRE_ORDER_DATE_TRUE");
      } else {
        this.$store.commit("order/PRE_ORDER_DATE_FALSE");
      }
    },
    deliveryMode: function (val) {
      this.$store.commit("order/SET_DELIVERY_MODE", val);
      console.log("deliveryMode", this.$store.state.order.deliveryMode);
    },

    // brands: function(val) {
    // },
  },
  computed: {
    ...mapState({
      // a: state => state.some.nested.module.a,
      orderId: (state) => state.order.orderId,
      customer: (state) => state.auth.customer,
      saveCards: (state) => state.payment.customerCards,
      addresses: (state) => state.address.addresses,
      fullAddr: (state) => state.address.full_name,
      streetId: (state) => state.address.name,
      loadingAddress: (state) => state.address.loadingStatus,
      loadingCustomer: (state) => state.auth.loadingCustomer,
      addressCity: (state) => state.address.region,
      showLogin: "loginProcess",
      addressState: "address",
      brands: "brands",
      selectedBrand: "selectedBrand",
      addToOrder: (state) => state.order.addsToOrder,
      favorite_dishes: (state) => state.order.favorite_dishes,
      checkDeliveryAddressModule: (state) => state.deliveryAddressModule,
      viewBtnChoosePreOrderDate: (state) => state.order.viewBtnChoosePreOrderDate,
      confirmationToken: (state) => state.order.confirmationToken,
      returnUrl: (state) => state.order.returnUrl,
      showWidget: (state) => state.order.showWidget,
      deliveryType: (state) => state.order.deliveryType,
    }),
    ...mapGetters({
      totalAmount: "order/totalAmount",
      getAmountCart: "order/getAmountCart",
      // getNearestPointId: 'order/getNearestPointId',
      // getActualAddress: 'address/getActualAddress',
      getOrderBrandId: "order/getOrderBrandId",
      minPriceExist: "order/minPriceExist",
      getMinPrice: "order/getMinPrice",
    }),
    getMinPriceDelivery() {
      const amountCart = this.getAmountCart;
      return this.getMinPrice - amountCart;
    },
    deliveryPriceCondition() {
      if (!this.homePageExist && !this.selfDeliveryTypeExist && this.orderPageExist) return true;
      return false;
    },
    homePageExist() {
      if (window.history.state.current === "/" + this.$route.params.brand) {
        return true;
      }
      return false;
    },

    selfDeliveryTypeExist() {
      console.log("test cart deliveryType", this.deliveryType);
      return Number(this.deliveryType) === 2 ? true : false;
    },
    orderPageExist() {
      if (window.history.state.current === "/order") {
        // при дозаказе считать доставку не нужно
        return true;
      }
      return false;
    },
    deliveryOrderPageExist() {
      if (window.history.state.current === "/deliveryOrder/" + this.$route.params.id) {
        // при дозаказе считать доставку не нужно
        return true;
      }
      return false;
    },
    pointClose() {
      if (this.preOrderTime[0] === "Закрыто") return true;
      return false;
    },
    breadCrumbsPath() {
      const brand = this.filterBrands;
      if (!Array.isArray(brand) || brand.length === 0) return [{ name: "Оформление заказа", path: null }];

      return [
        { name: brand[0].name, path: brand[0].name_translit },
        { name: "Оформление заказа", path: null },
      ];
    },
    getActualAddress() {
      return this.addresses.find((addr) => {
        if (addr.actual === true) return addr;
      });
    },
    editNumberLock() {
      return 0 < this.$store.state.auth.globalTimer && this.$store.state.auth.globalTimer < 60;
    },
    getAddressWithoutCity() {
      let addressList = [];

      this.addresses.forEach((addr) => {
        const copy = Object.assign({}, addr);
        addressList.push(copy);
      });

      addressList.forEach((addr) => {
        const position = addr.full_addr.indexOf(",");
        addr.full_addr = addr.full_addr.slice(position + 2);
      });
      return addressList;
    },

    validationPhoneNumber() {
      if (this.phoneNumber && this.phoneNumber.length === 18) {
        return true;
      }
      return false;
    },

    // предзаказ времени
    preOrderTime() {
      console.log("start preOrderTime");

      // ? не понятно какую дату тут будут выставлять точки. Это может быть либо время доставки, но работают раньше, либо это начало работы точки
      // ? если работают раньше оставить как есть
      // ? если это начало работы точки, тогда всегда прибавлять час к доставке
      // со скольки точка развозит заказы?
      // ?CONFIG вытащить значения в config

      // узнаем, есть ли расписание на текущий день
      const workTime = this.workDayTime;

      if (!workTime) {
        //заблокируем кнопку оплатить
        this.$store.commit("order/PRE_ORDER_DATE_TRUE");
        return ["Выходной"];
      }
      console.log("workTime preOrderTime", workTime);
      this.$store.commit("order/PRE_ORDER_DATE_FALSE");
      // выставляем начало и окончание работы точки
      const pointStartWorkDelivery = workTime ? workTime.from : this.startWorkPoint;
      const pointEndWorkDelivery = workTime ? workTime.to : this.endWorkPoint;

      if (!pointStartWorkDelivery && !pointEndWorkDelivery) return false;
      // создаем даты
      const today = this.timeNow ? new Date(this.timeNow) : new Date();
      const startDelivery = new Date();
      const select = new Date(this.selectDay);

      // подготавливаем данные
      const startHours = Number(pointStartWorkDelivery.split(":")[0]);
      const startMinutes = Number(pointStartWorkDelivery.split(":")[1]);
      const EndHours = startHours > Number(pointEndWorkDelivery.split(":")[0]) ? Number(pointEndWorkDelivery.split(":")[0]) + 24 : Number(pointEndWorkDelivery.split(":")[0]);
      const EndMinutes = Number(pointEndWorkDelivery.split(":")[1]);

      console.log("startHours/EndHours", startHours, EndHours);

      // интервал времени (ВАЖНО! hoursCheck % intervalTimeMin === 0)
      const intervalTimeMin = 15;
      //время доставки мин
      const timeDelivery = 30;
      //время готовки точки

      const timeCooking = this.getCooking_time ? this.getCooking_time : 30;
      // для проверки интервала
      const hoursCheck = 60;

      if (hoursCheck % intervalTimeMin !== 0) {
        return console.error("интервал не делится на час");
      }
      // высчитываем количество частей интервала в часе
      const countIntervalInHour = hoursCheck / intervalTimeMin;
      let hoursNow;
      let minutesNow;
      let countWorkHoursPoint;
      // круглосуточная работа точки
      const aroundTheClock = EndHours - startHours === 0 ? true : false;

      // нужно определить, это заказ или предзаказ
      // если предзаказ, расчитываем на другие дни, иначе расчет на сегодня
      const selectDay = select.toISOString().slice(0, 10);
      const nowDay = today.toISOString().slice(0, 10);

      if (selectDay > nowDay) {
        hoursNow = startHours;
        minutesNow = startMinutes;

        startDelivery.setMinutes(0);

        // количество рабочих часов точки
        countWorkHoursPoint = aroundTheClock ? 24 : EndHours - startHours < 0 ? 24 + EndHours - startHours : EndHours - startHours;
      } else {
        // расчет после полуночи (это условие сработает, если preOrderDays увидит что расписание вчерашнего дня еще не закончилось (сместит весь массив дат на один день назад, для того что бы preOrderTime рассчитал расписание на вчерашний день))
        let getHours = null;
        if (selectDay < nowDay) {
          hoursNow = today.getHours() + 24;
          minutesNow = today.getMinutes();
          getHours = today.getHours() + 24;
        } else {
          hoursNow = today.getHours();
          minutesNow = today.getMinutes();
          getHours = today.getHours() < startHours ? startHours : today.getHours();
        }

        // количество оставшихся часов точки
        countWorkHoursPoint = aroundTheClock ? 24 : EndHours - getHours < 0 ? 0 : EndHours - getHours;
      }

      // если утро, считай часы с начала работы точки
      const hoursDelivery = hoursNow < startHours ? startHours : hoursNow;
      const minutesDelivery = hoursNow < startHours ? startMinutes : minutesNow;
      console.log("час сейчас", today.getHours());
      console.log("TimeNow", this.timeNow);
      console.log("время сейчас", today);
      console.log("время начала работы точки", startHours);
      console.log("начало времени массива", hoursDelivery, minutesDelivery);

      startDelivery.setHours(hoursDelivery);
      startDelivery.setMinutes(minutesDelivery);

      // Если время сейчас больше чем pointEndWorkDeliveryTime - cookingTime (изменим предзаказ на ближайший)
      // время окончания работы точки
      const pointEndWorkDeliveryTime = new Date(this.selectDay);
      pointEndWorkDeliveryTime.setHours(EndHours);
      pointEndWorkDeliveryTime.setMinutes(EndMinutes);
      // время приготовления блюда
      const pointEndWorkDeliveryTimeMinusCookingTime = new Date(pointEndWorkDeliveryTime.getTime() - timeCooking * 60000);
      console.log("pointEndWorkDeliveryTime", pointEndWorkDeliveryTime);
      console.log("pointEndWorkDeliveryTimeMinusCookingTime", pointEndWorkDeliveryTimeMinusCookingTime);
      console.log("today", today);

      if (today.getTime() >= pointEndWorkDeliveryTime.getTime()) {
        return (timeArray = ["Закрыто"]);
      } else if (today >= pointEndWorkDeliveryTimeMinusCookingTime && today <= pointEndWorkDeliveryTime) {
        return (timeArray = ["Ближайший"]);
      }

      // рассчитаем первый ближайший предзаказ firstNearestDelivery

      // определим время первого ближайшего предзаказа
      const timeNearestDelivery = new Date(startDelivery.getTime() + (timeCooking + timeDelivery) * 60000);

      // захватим сырые минуты, и округлим их до ближайшего интервала
      const rawMinutes = timeNearestDelivery.getMinutes();
      // находим хвост, не вписывающийся в интервал
      const tail = rawMinutes % intervalTimeMin;
      // основное время
      const baseTime = rawMinutes - tail;
      // прибавляем к основному один интервал, что бы закрыть хвост, если он есть.
      const minutesNearestDelivery = baseTime + (tail === 0 ? 0 : intervalTimeMin);
      // сбрасываем сырое время
      timeNearestDelivery.setMinutes(0);

      // устанавливаем время ближайшей доставки
      const firstNearestDelivery = new Date(timeNearestDelivery.getTime() + minutesNearestDelivery * 60000);
      console.log("firstNearestDelivery", firstNearestDelivery);

      // массив минут
      let timeArray = [];

      // рассчитаем количество итераций для цикла при создании списка времени

      // количество интервала в оставшихся часах работы
      const countIntervalsOnHours = countWorkHoursPoint * countIntervalInHour;
      // количество интервалов в минутах ближайшей доставки
      const minutesHoursIntervals = minutesNearestDelivery / intervalTimeMin + 1;
      // остаток минут в конце рабочего дня если есть (должен быть целым числом)
      const minutesIntervals = EndMinutes % intervalTimeMin === 0 ? EndMinutes / intervalTimeMin : (EndMinutes - (EndMinutes % intervalTimeMin)) / intervalTimeMin;
      // количество интервалов на доставку и готовку
      // const deliveryIntervals = ((timeCooking + timeDelivery) % intervalTimeMin) > 0 ?
      // // убираем хвост что бы делился без остатка
      // (((timeCooking + timeDelivery) - ((timeCooking + timeDelivery) % intervalTimeMin)) / intervalTimeMin):
      // // делится без остатка
      // (timeCooking + timeDelivery) / intervalTimeMin
      // const cookingTimeIntervals = ((timeCooking ) % intervalTimeMin) > 0 ?
      // // убираем хвост что бы делился без остатка
      // ((((timeCooking ) - ((timeCooking ) % intervalTimeMin)) / intervalTimeMin) + 1):
      // // делится без остатка
      // (timeCooking ) / intervalTimeMin
      // // Ближайший заказ в предзаказе наступает, если до окончания времени работы остается меньше времени чем на готовку основного блюда
      // if(countIntervalsOnHours + minutesIntervals <= cookingTimeIntervals) {
      //     return timeArray = ['Ближайший']
      // }

      // количество интервалов за рабочий день (убрал для теста : (this.selectDay > today ? - deliveryIntervals : 0 ) + deliveryIntervals)
      const countIntervalOnWorkDay = countIntervalsOnHours + minutesIntervals - minutesHoursIntervals;
      console.log("countIntervalOnWorkDay", countIntervalOnWorkDay);

      for (let index = 0; index < countIntervalOnWorkDay; index++) {
        let date = new Date(firstNearestDelivery.getTime() + 15 * 60000 * index);
        const time = date.toTimeString().slice(0, 5);
        timeArray.push(time);
      }
      if (timeArray.length === 0) {
        timeArray = ["Закрыто"];
      }
      return timeArray;
    },
    // предзаказ дня
    preOrderDays() {
      // на сколько дней вперед считать предзаказ
      const daysCount = 20;
      // const selectDay = new Date()
      // const selectedDay = selectDay(this.selectDay)
      // console.log('selectedDay', this.workDayTime)

      // собирающий массив
      let daysArray = [];
      const workWeekTime = this.workWeekTime;
      if (!workWeekTime.length) return;
      let pointWork = false;

      //создаем даты на 20 дней вперед
      for (let day = 0; day < daysCount; day++) {
        const date = new Date();
        date.setDate(date.getDate() + day);
        // расписание на неделю
        // определим день недели в цикле
        const weekDay = Number(date.getDay()) - 1 === -1 ? 6 : date.getDay() - 1;
        // если время после полуночи, есть шанс что точка еще работает
        const nowHours = date.getHours();
        const nowMinutes = date.getMinutes();
        // console.log('nowHours', nowHours)
        if (nowHours <= 7) {
          // узнаем расписание предыдущего дня, если точка все еще работает, откатим массив на один день назад
          const preDay = weekDay - 1 === -1 ? 6 : weekDay - 1;
          const preWorkTime = workWeekTime[preDay];

          // const newTimeFrom = Number(day.from.split(':')[0])
          // const oldTimeFrom = Number(weekTime[i].from.split(':')[0])

          // // пересчет окончания рабочего дня на формат (00:00 = 24, 01:00= 25)
          // // пример работы точки: (10:00 - 01:00) -> (1 - 10 < 0) => 24 + 1 = 25 === (10:00 - 25:00)
          // const oldTimeTo = Number(weekTime[i].to.split(':')[0]) - oldTimeFrom < 0 ? (24 + Number(weekTime[i].to.split(':')[0])) : Number(weekTime[i].to.split(':')[0])
          // const newTimeTo = Number(day.to.split(':')[0]) - newTimeFrom < 0 ? (24 + Number(day.to.split(':')[0])) : Number(day.to.split(':')[0])

          console.log("preWorkTime", preWorkTime);
          const workTimeTo = Object.keys(preWorkTime).length === 0 ? false : Number(preWorkTime.to.split(":")[0]);
          const workTimeFrom = Object.keys(preWorkTime).length === 0 ? false : Number(preWorkTime.from.split(":")[0]);
          const preHours = Object.keys(preWorkTime).length === 0 ? false : workTimeTo - workTimeFrom < 0 ? 24 + workTimeTo : workTimeTo;
          const preMinutes = Object.keys(preWorkTime).length === 0 ? false : Number(preWorkTime.to.split(":")[1]);

          console.log("nowHours", nowHours + 24);
          console.log("nowMinutes", nowMinutes);
          console.log("preHours", preHours);
          console.log("preMinutes", preMinutes);

          if ((preHours && preHours > nowHours + 24) || (preHours && preHours === nowHours + 24 && preMinutes > nowMinutes)) {
            console.log("check minus day");
            // date.setDate(date.getDate() - 1)
            pointWork = true;
          }
          if (pointWork) {
            date.setDate(date.getDate() - 1);
          }
        }

        // +1 день
        // if (preWorkDay && this.selectDay === 'Сегодня') date.setDate(date.getDate())
        // формируем дату и день недели
        const dayMonthText = date.toLocaleString("ru", { day: "numeric", month: "long" });
        const weekDayText = date.toLocaleString("ru", { weekday: "short" });
        let dayOff;

        if (workWeekTime) {
          // определим день недели в цикле
          // const weekDay = Number(date.getDay()) - 1 === -1 ? 6 : date.getDay() - 1
          // если ключей у объекта нет, значит выходной
          dayOff = Object.keys(workWeekTime[weekDay]).length == 0 ? true : false;
        }

        if (dayOff) {
          continue;
        }

        const dayNow = day === 0 ? "Сегодня" : day === 1 ? "Завтра" : `${dayMonthText} (${weekDayText})`;

        const title = `${dayNow}${dayOff ? dayOff : ""}`;
        daysArray.push({
          title,
          date,
        });
      }
      return daysArray;
    },

    // если сегодня выходной или уже закрыто, запретить ближайший заказ
    // todayDayOff(){
    //     return this.preOrderTime[0] === 'Закрыто' || this.preOrderDays[0].title !== 'Сегодня'  ? true : false
    // },
    // дата предзаказа
    preOrderDate() {
      const time = this.selectTime;
      const day = this.selectDay;

      console.log("preOrderDate", day);
      console.log("preOrderDate", time);

      if (!time || !day) {
        return;
      }

      if (time === "Закрыто" || time === "Выходной") {
        this.$store.commit("order/PRE_ORDER_DATE_TRUE");
        return;
      } else {
        this.$store.commit("order/PRE_ORDER_DATE_FALSE");
      }

      const now = new Date();
      let date = new Date(day);
      // let nowTime = new Date(date.getTime() +  (this.getCooking_time + 30) * 60000)
      console.log("Number(time === ", Number(time === "Ближайший" ? this.getCooking_time + 30 : time.split(":")[0]));

      date.setHours(Number(time === "Ближайший" ? now.getHours() : time.split(":")[0]));
      date.setMinutes(Number(time === "Ближайший" ? now.getMinutes() + (this.getCooking_time + 30) : time.split(":")[1]));
      date.setSeconds(0);
      date.setMilliseconds(0);
      console.log("start date", date);

      // проблема (не правильно отправляет дату заказа, если точка работает после полуночи)
      // сверим даты
      const nowHours = now.getHours();
      const orderHours = Number(time === "Ближайший" ? now.getHours() : time.split(":")[0]);

      console.log("orderHours < 6 && (date.getDate() < now.getDate())", orderHours, orderHours < 6, date.getDate(), now.getDate(), date.getDate() < now.getDate());

      // если сейчас час больше чем в заказе, значит это пограничный случай. Точка работает после полуночи. Прибавить +1 день к заказу
      if (orderHours <= 6 && date.getDate() < now.getDate()) {
        console.log(nowHours > orderHours);
        console.log("before set", date);
        date.setTime(date.getTime() + 24 * (60 * 60000)); // +24 часа
      }
      console.log("after", date);

      return date.toISOString();
    },
    // фильтруем бренды исходя из нашей корзины
    filterBrands() {
      if (this.brands && this.getOrderBrandId) {
        return this.brands.filter((brand) => this.getOrderBrandId.includes(brand.id));
      } else {
        return [];
      }
    },
    // все точки в нашей корзине (тут могут быть точки и других брендов, в будущем)
    brandPoints() {
      if (!this.filterBrands) return [];
      // this.deliveryMode  (1- ближайший, 2 - предзаказ)
      // this.workWeekTime (график работы на неделю у бренда)
      let points = [];
      this.filterBrands.forEach((brand) => {
        brand.points.forEach((point) => {
          let pointBrand = {
            id: point.id,
            delivery_id: point.delivery_id,
            address: point.city ? point.city + ", " + point.address : point.address,
          };
          // сделать проверку на время работы
          points.push(pointBrand);
        });
      });
      return points;
    },
    getPoint() {
      // console.log('getPoint getter start')
      if (!this.filterBrands) return [];

      let brandPointDelivery = [];
      // // если выбран самовывоз
      // if(this.selfDelivery){
      //     return brandPointDelivery.push(this.pointSelfDelivery)
      // }
      const filterBrands = this.filterBrands;
      console.log("filterBrands getPoint", filterBrands);
      filterBrands.forEach((brand) => {
        brand.points.forEach((point) => {
          if (brand.nearest_point === point.id) {
            const city = point.city ? point.city + ", " : "";
            const addr = {
              pointId: point.delivery_id,
              addressPoint: city + point.address,
            };
            brandPointDelivery.push(addr);
          }
        });
      });
      console.log("brandPointDelivery", brandPointDelivery);
      return brandPointDelivery;
    },
    isWorkPoint(point, date) {
      let now = new Date(date);
      let time_now = { h: now.getHours(), m: now.getMinutes() };
      let work = true;
      if (typeof point.settings === "string") point.settings = JSON.parse(point.settings);

      if (point.settings.work_time) {
        // узнаем день недели
        let day = now.getDay() - 1;
        day = day === -1 ? 6 : day;
        // узнаем расписание точки на день недели
        let time = point.settings.work_time[day];
        let time_point;
        if (time.from && time.to) time_point = { from: { h: time.from.split(":")[0], m: time.from.split(":")[1] }, to: { h: time.to.split(":")[0], m: time.to.split(":")[1] } };
        if (time_point && time_point.to && time_point.to.h && Number(time_point.to.h) === 0) time_point.to.h = 24;
        if (time.from && time.to && time_now.h >= time_point.from.h && time_now.m >= time_point.from.m && time_now.h < time_point.to.h) {
          work = true;
        } else work = false;
      } else {
        let time_point = {
          from: { h: point.settings.start_time_work.split(":")[0], m: point.settings.start_time_work.split(":")[1] },
          to: { h: point.settings.end_time_work.split(":")[0], m: point.settings.end_time_work.split(":")[1] },
        };
        if (time_now.h >= time_point.from.h && time_now.m >= time_point.from.m && time_now.h < time_point.to.h) work = true;
        else work = false;
      }
      console.log("isWorkPoint", point.delivery_id, work, now.toLocaleString("ru", { day: "numeric", month: "long", hour: "numeric", minute: "numeric" }));
      return work;
    },
    getCooking_time() {
      let time;
      if (this.brandTimeCooking.min && this.brandTimeCooking.max) time = (this.brandTimeCooking.min + this.brandTimeCooking.max) / 2;
      if (this.brandTimeCooking.min && !this.brandTimeCooking.max) time = this.brandTimeCooking.min;
      if (!this.brandTimeCooking.min && !this.brandTimeCooking.max) time = 0;
      return time;
    },
    brandTimeCooking() {
      if (this.filterBrands && this.filterBrands.length === 1) {
        return this.filterBrands[0].settings.cooking_time;
      }
      return false;
    },
    //график работы на неделю
    workWeekTime() {
      if (!this.filterBrands) return false;
      if (this.deliveryType == 2 && !this.pointSelfDelivery) return false;

      const filterBrands = this.filterBrands;
      console.log("filterBrands on workWeekTime", filterBrands);
      console.log("this.deliveryType", this.deliveryType);
      console.log("this.deliveryType", this.pointSelfDelivery);

      let weekTime = [];
      filterBrands.forEach((brand) => {
        const pointDelivery = this.deliveryType == 2 ? this.pointSelfDelivery.id : brand.point_ids;
        brand.points.forEach((point) => {
          if (Array.isArray(pointDelivery) ? pointDelivery.includes(point.id) : pointDelivery === point.id) {
            if ("work_time" in point.settings === false) {
              point.settings.work_time = [];
              for (let i = 0; i < 7; i++) {
                point.settings.work_time.push({ from: point.settings.start_time_work, to: point.settings.end_time_work });
              }
            }
            // перебираем расписание на неделю всех точек
            point.settings.work_time.forEach((day, i) => {
              if (weekTime.length < 7) {
                // пропускаем, если выходной
                if (Object.keys(day).length === 0) {
                  weekTime.push({});
                  return;
                }

                weekTime.push({ from: day.from, to: day.to });
              } else {
                // пропускаем, если выходной
                if (Object.keys(day).length === 0) return;

                // переназначение
                if (Object.keys(weekTime[i]).length === 0) {
                  weekTime[i] = { from: day.from, to: day.to };
                } else {
                  const newTimeFrom = Number(day.from.split(":")[0]);
                  const oldTimeFrom = Number(weekTime[i].from.split(":")[0]);

                  // пересчет окончания рабочего дня на формат (00:00 = 24, 01:00= 25)
                  // пример работы точки: (10:00 - 01:00) -> (1 - 10 < 0) => 24 + 1 = 25 === (10:00 - 25:00)
                  const oldTimeTo = Number(weekTime[i].to.split(":")[0]) - oldTimeFrom < 0 ? 24 + Number(weekTime[i].to.split(":")[0]) : Number(weekTime[i].to.split(":")[0]);
                  const newTimeTo = Number(day.to.split(":")[0]) - newTimeFrom < 0 ? 24 + Number(day.to.split(":")[0]) : Number(day.to.split(":")[0]);

                  if (newTimeTo > oldTimeTo) weekTime[i].to = day.to;
                  if (newTimeFrom > oldTimeFrom) weekTime[i].from = day.from;
                }
              }
            });
          }
        });
      });
      // TODO выход должен быть [{from: '09:00', to:'22:00'}] ? [{from: '09:00', to:'22:00'}] : false
      return weekTime ? weekTime : false;

      // ! старый вариант
      //  console.log('test weekTime', weekTime)
      // filterBrands.forEach(brand => {
      //     //поменять точку, если выбран самовывоз
      //     const pointDelivery = this.deliveryType == 2 ? this.pointSelfDelivery.id : brand.nearest_point
      //     brand.points.forEach(point => {
      //         if (pointDelivery === point.id) {
      //             if(point.settings.work_time !== undefined) workWeekTime = point.settings.work_time
      //             else {
      //                 point.settings.work_time = []
      //                 for (let i = 0; i < 7; i++) {
      //                     point.settings.work_time.push({from: point.settings.start_time_work, to: point.settings.end_time_work})
      //                 }
      //                 workWeekTime = point.settings.work_time
      //             }
      //         }
      //     })
      // })
    },
    //график работы на сегодня
    workDayTime() {
      if (!this.filterBrands) return false;
      console.log("start workDayTime");
      let workTime = false;

      const workWeekTime = this.workWeekTime;
      console.log("workWeekTime", workWeekTime);
      if (workWeekTime.length) {
        //определяем день недели
        const selectDay = new Date(this.selectDay);
        const weekDay = Number(selectDay.getDay()) - 1 === -1 ? 6 : selectDay.getDay() - 1;

        if (Object.keys(workWeekTime[weekDay]).length == 0) {
          return false;
        }

        return workWeekTime[weekDay];
      }

      return workTime;
    },
    // проверяем, закрыт ли сейчас ресторан
    restaurantClose() {
      if (!this.filterBrands) return false;
      let workTime = false;

      const workWeekTime = this.workWeekTime;
      if (workWeekTime.length) {
        const today = this.timeNow ? new Date(this.timeNow) : new Date();
        // день недели
        const weekDay = Number(today.getDay()) - 1 === -1 ? 6 : today.getDay() - 1;
        // расписание
        const scheduleTimeToday = workWeekTime[weekDay];
        // если пустые, значит сегодня выходной
        if (Object.keys(scheduleTimeToday).length == 0) {
          return true;
        }
        const EndHoursCheduleToday =
          Number(scheduleTimeToday.to.split(":")[0]) - 24 < 0 ? Number(scheduleTimeToday.to.split(":")[0]) + 24 : Number(scheduleTimeToday.to.split(":")[0]);
        const EndMinutesCheduleToday = scheduleTimeToday.to.split(":")[1];
        const scheduleToday = new Date();

        scheduleToday.setHours(EndHoursCheduleToday);
        scheduleToday.setMinutes(EndMinutesCheduleToday);

        const close = today > scheduleToday;

        return close;
      }

      return workTime;
    },
    startWorkPoint() {
      if (!this.filterBrands) return false;

      let startWork;
      const filterBrands = this.filterBrands;
      filterBrands.forEach((brand) => {
        brand.points.forEach((point) => {
          if (brand.nearest_point === point.id) {
            startWork = point.settings.start_time_work;
          }
        });
      });
      return startWork;
    },
    endWorkPoint() {
      if (!this.filterBrands) return false;

      let endWork;
      const filterBrands = this.filterBrands;
      filterBrands.forEach((brand) => {
        brand.points.forEach((point) => {
          if (brand.nearest_point === point.id) {
            endWork = point.settings.end_time_work;
          }
        });
      });
      return endWork;
    },
    toManyBrands() {
      if (this.filterBrands.length > 1) {
        return true;
      }
      return false;
    },

    // noEmptyAddress() {
    //     if( this.addressState.addresses.length > 0 && this.selectedAddress.id) return ''
    //     if( (this.addressState.addresses.length > 0 && !this.selectedAddress.id) && !this.showForm) return 'Выберете адрес доставки'
    //     // if( "initialState.status.loggedIn")
    //     if( this.street === '') return "Заполните улицу"
    //     if( this.entrance === null) return "Заполните подъезд"
    //     if( this.apartment === null) return "Заполните квартиру"
    //     if( this.floor === null) return "Заполните этаж"
    //     return ''
    // },

    // noEmptyNumber(){
    //     if (!this.phoneNumber) return 'Заполните номер телефона'
    //     return ''
    // },

    showAddAddress() {
      if (this.hasMyAddress !== true || this.showForm === true || Number(this.deliveryType) === 3) return true;
      return false;
    },

    deliveryTimeInfo() {
      if (Number(this.deliveryType) === 2) {
        return "КОГДА ПЛАНИРУЕТЕ ЗАБРАТЬ";
      }
      return "ВРЕМЯ ДОСТАВКИ";
    },

    addressInfo() {
      if (this.hasMyAddress === true) {
        return "МОИ АДРЕСА";
      }
      return "АДРЕС ДОСТАВКИ";
    },

    hasMyAddress() {
      if (Number(this.deliveryType) === 1 && this.addresses.length && this.customer) return true;

      return false;
    },

    selfDelivery() {
      if (Number(this.deliveryType) === 2) {
        return true;
      }

      return false;
    },

    currentCustomerPhone() {
      if (this.customer && this.customer.number === this.phoneNumber) {
        return true;
      }
      return false;
    },

    isDisabled() {
      if (!this.checkUserAgreement || !this.validationPhoneNumber) {
        return true;
      }
      return false;
    },
    // totalCost(){
    //     let amount = 0
    //     this.$store.state.order.cartItems.forEach(item => {
    //         amount += item.price
    //     });
    //     return `${amount} ₽`
    // }
    addToOrderShow() {
      return this.addToOrder ? this.addToOrder.slice(0, 3) : [];
    },
  },
  methods: {
    ...mapActions({
      rememberCustomerName: "auth/setCustomerName",
      getCustomerName: "auth/getCustomerName",
    }),
    // скрол в начало
    scrollStart() {
      console.log("test scroll order start");
      //document.documentElement.scrollIntoView(true)
      $("html, body").animate(
        {
          scrollTop: 0,
        },
        {
          duration: 370, // по умолчанию «400»
          easing: "linear", // по умолчанию «swing»
        }
      );
    },
    openMap() {
      this.$store.commit("SHOW_DELIVERY_ADDRESS_MODULE");
    },
    onClickInput(e) {
      this.errorPhone = null;
      this.errorPhoneAuth = null;

      if (this.editNumberLock) {
        e.preventDefault();
        console.log();
        this.showInputDelayError = true;
        setTimeout(() => {
          this.showInputDelayError = false;
        }, 3000);
      }
    },
    //   testMainHeight(){
    //     const a = document.querySelector("#app > div > div.container-fluid.p-0 > div > div.d-flex > div.d-flex.main-order")
    //     const b = document.querySelector(".trash-position")
    //     console.log('mainHeight = ', a)
    //     console.log('mainHeight test = ', b)
    //     if(a){
    //         return this.mainHeight = b.getBoundingClientRect().height
    //     }
    //     return this.mainHeight  = 550
    // },
    validationLogin() {
      if (this.isDisabled) {
        return this.checkOrder();
      }
    },
    selectOptionsItems(time) {
      this.showSelectOptions === false;
      this.selectTime === time;
    },
    scrollBrands() {
      let cart_bottom = 0;
      let check_cart = true;
      window.onscroll = () => {
        // корзина
        let footer_top = document.querySelector("footer").getBoundingClientRect().top,
          cart = document.querySelector(".trash");
        // cart = document.querySelector('.trash-position')
        if (cart) {
          if (check_cart) cart_bottom = cart.getBoundingClientRect().bottom;
          let sm = cart_bottom - footer_top;
          if (sm > 0) {
            cart.style.top = 80 - sm + "px";
            check_cart = false;
          }
          if (sm <= 0) {
            cart.style.top = 150 + "px";
          }
        }
      };
    },
    async getPointsByBrandsId() {
      try {
        const filterBrandsWithPoints = this.filterBrands;

        for (let brand of filterBrandsWithPoints) {
          //brand.points = []

          /*let check_nearest_point = true
                        // находим точки бренда
                        for (let point of brand.delivery_point_ids){
                            const getPoint = await this.$store.dispatch("getPoint", point)
                            console.log('getPoint = ', getPoint)
                            // добавляем точку к бренду
                            if (getPoint.success && getPoint.point) brand.points.push(getPoint.point)
                            // добавляем точку как ближайшую
                            if (check_nearest_point) brand.nearest_point = point
                            // проверяем работу точки
                            if (this.checkTimeWork(brand).work) check_nearest_point = false
                        }*/

          console.log("brand test", brand);

          let check_nearest_point = true;
          for (let point of brand.points) {
            if (check_nearest_point) brand.nearest_point = point.id;
            if (this.checkTimeWork(brand).work) check_nearest_point = false;
          }
        }

        this.filterBrandsWithPoints = filterBrandsWithPoints;

        // находим бренд
        // console.log('getBrandById start = ', id)
        // const getBrand = await this.$store.dispatch("brandById",id)
        // const brand = getBrand.item

        /*if (!this.isUpgradeAddsToOrder){
                        this.$store.dispatch('order/upgradeAddsToOrder', brand)
                        this.isUpgradeAddsToOrder = true
                    }*/
        // //находим расписание ближайшей точки
        // const nearestPoint = brand.nearest_point
        // const point = brand.points.find(point => {
        //     point.id === nearestPoint
        // })
        // const start = point.settings.start_time_work
        // const end = point.settings.end_time_work
        // console.log('test time point', start, end)

        //this.$store.commit('brands', brand)
      } catch (error) {
        console.error(error);
      }
    },
    getBrands() {
      this.$store.dispatch("brands").then(
        (data) => {
          const brands = data.items;
          for (let elem of brands) {
            elem.nearest_point = elem.points.length ? elem.points[0].id : 0; // elem.point_ids[0]
            // if (elem.type === "food") this.filters.food = this.filters.food.concat(elem.subtype)
            // if (elem.type === "flowers") this.filters.flowers = this.filters.flowers.concat(elem.subtype)
            // if (elem.type === "cosmetics") this.filters.cosmetics = this.filters.cosmetics.concat(elem.subtype)
            elem.entity_info = JSON.parse(elem.entity_info);
            if (typeof elem.settings == "string") elem.settings = JSON.parse(elem.settings);

            /*elem.points = []

                            let check_nearest_point = true
                            for (let point of elem.delivery_point_ids){
                                console.log('point Id = ', point)
                                this.$store.dispatch("getPoint", point).then(
                                    res => {
                                        console.log('res point', res.point)
                                        if (res.success && res.point) elem.points.push(res.point)
                                        if (check_nearest_point) elem.nearest_point = point
                                        if (this.checkTimeWork(elem).work) check_nearest_point = false
                                    },
                                    error => {
                                        console.error(error)
                                    }
                                )
                            }*/
            let check_nearest_point = true;
            for (let point of elem.points) {
              if (check_nearest_point) elem.nearest_point = point.id;
              if (this.checkTimeWork(elem).work) check_nearest_point = false;
            }

            for (let dish of elem.dishes) {
              dish.settings = JSON.parse(dish.settings);
            }
          }
          this.$store.commit("brands", brands);
        },
        (error) => {
          console.error(error);
        }
      );
    },
    // Проверка времени работы (сравнивает с текущим)
    checkTimeWork(brand) {
      let now = new Date();
      let time_now = { h: now.getHours(), m: now.getMinutes() };
      let el = { work: true, pre_order: "" };
      if (!brand.points) return el;
      let point = brand.points.find((el) => el.id == brand.nearest_point);
      if (!point) return el;
      if (point.settings.work_time) {
        let day = now.getDay() - 1;
        day = day == -1 ? 6 : day;
        let time = point.settings.work_time[day];
        let time_point;
        if (time.from && time.to) time_point = { from: { h: time.from.split(":")[0], m: time.from.split(":")[1] }, to: { h: time.to.split(":")[0], m: time.to.split(":")[1] } };
        if (time_point && time_point.to && time_point.to.h && Number(time_point.to.h) == 0) time_point.to.h = 24;
        if (time.from && time.to && time_now.h >= time_point.from.h && time_now.m >= time_point.from.m && time_now.h < time_point.to.h) {
          el.work = true;
        } else {
          if (time.from && time.to && time_now.h < time_point.from.h) {
            el.pre_order = "Сегодня с " + time.from + " до " + time.to;
            el.work = false;
          } else {
            el.work = false;
            day = day == 6 ? 0 : day + 1;
            time = point.settings.work_time[day];
            if (time.from && time.to) el.pre_order = "Завтра с " + time.from + " до " + time.to;
            else {
              day = day == 5 ? 0 : day + 2;
              time = point.settings.work_time[day];

              if (time === undefined) return (el.pre_order = "Послезавтра с --:-- до --:--");
              if (time.from && time.to) el.pre_order = "Послезавтра с " + time.from + " до " + time.to;
            }
          }
        }
      } else {
        let time_point = {
          from: { h: point.settings.start_time_work.split(":")[0], m: point.settings.start_time_work.split(":")[1] },
          to: { h: point.settings.end_time_work.split(":")[0], m: point.settings.end_time_work.split(":")[1] },
        };
        if (time_now.h >= time_point.from.h && time_now.m >= time_point.from.m && time_now.h < time_point.to.h) el.work = true;
        else {
          el.work = false;
          el.pre_order = "Завтра с " + point.settings.start_time_work + " до " + point.settings.end_time_work;
        }
      }

      return el;
    },
    setMapPosition(e) {
      this.saveAddressName();
      this.$store.state.address.coord = [e.lon, e.lat];
    },
    saveAddressName() {
      let input = document.querySelector("#new-addr-input");
      this.street = input.value;
      this.$store.state.address.name = this.street;
      this.$store.state.address.id = null;
      if (this.addressState.name == "") this.$store.state.address.coord = [];
    },
    // checkValidFormDate(){
    //     if(Number(this.deliveryMode) === 2){
    //     const textError = 'Поле обязательно'
    //     if(!this.selectTime) this.errorTime = textError
    //     if(!this.selectDay) this.errorDay = textError

    //         if(this.errorTime ||
    //             this.errorDay
    //             ){
    //                 return false
    //             }
    //         return true
    //     }
    //     return true

    // },
    // checkValidFormPhone(){
    //     if(this.deliveryType === 1) {
    //         const textError = 'Поле обязательно'
    //         const textAuthError = 'Авторизуйтесь через смс'

    //         if(!this.phoneNumber) this.errorPhone = textError
    //         if(!this.customer) this.errorPhoneAuth = textAuthError
    //         // if(!this.customerName) this.errorName = textError

    //         if(this.errorPhone ||
    //             this.errorPhoneAuth
    //             // this.errorName ||
    //             ) {
    //                 return false
    //             }
    //         return true

    //     }
    //     if(this.deliveryType === 3) {
    //         const textError = 'Поле обязательно'
    //         const textAuthError = 'Авторизуйтесь через смс'

    //         if(!this.phoneNumber) this.errorPhone = textError
    //         if(!this.customer) this.errorPhoneAuth = textAuthError
    //         // if(!this.customerName) this.errorName = textError

    //         if(!this.anotherCustomerPhone) this.errorAnotherPhone = textError
    //         if(!this.anotherCustomerName) this.errorAnotherName = textError

    //         if(this.errorPhone ||
    //             this.errorPhoneAuth ||
    //             // this.errorName ||
    //             this.errorAnotherPhone ||
    //             this.errorAnotherName
    //             ) {
    //                 return false
    //             }
    //         return true

    //     }

    // },
    // checkValidFormAddress(){
    //     const textError = 'Поле обязательно'
    //     const textAuthError = 'Авторизуйтесь через смс'

    //     if(!this.street) this.errorAddress = textError
    //     if(!this.entrance) this.errorEntrance = textError
    //     if(!this.apartment) this.errorApartment = textError
    //     if(!this.floor) this.errorFloor = textError

    //     if(this.errorAddress ||
    //         this.errorEntrance ||
    //         this.errorFloor ||
    //         this.errorApartment) {
    //             return false
    //         }
    //     return true
    // },

    checkForm() {
      const textError = "Ошибка";
      const incorrectError = "Номер телефона не корректный";
      const noConfirmedError = "Подтвердите соглашение и авторизуйтесь";
      const authError = "Авторизуйтесь через смс";
      const authNoConfirmedError = "Не совпадает с подтвержденным, подтвердите соглашение и авторизуйтесь через смс заново";
      const authAuthError = "Не совпадает с подтвержденным, авторизуйтесь заново";
      const preOrderTimeError = "Точка сейчас закрыта, выберете другую точку или дату прездаказа";

      // не авторизован
      const noAuth = !this.customer ? true : false;
      // не корректный
      const incorrect = !this.validationPhoneNumber;
      // не подтвержден
      const noConfirmed = !this.checkUserAgreement;
      // автризован, но не авторизованный номер
      const noAuthNumber = !this.currentCustomerPhone;

      //тип ошибки

      // поля о госте
      this.errorPhoneAuth = incorrect
        ? incorrectError
        : noAuth && noConfirmed
        ? noConfirmedError
        : noAuth
        ? authError
        : noAuthNumber && noConfirmed
        ? authNoConfirmedError
        : noAuthNumber
        ? authAuthError
        : null;
      // if(!this.customerName) this.errorName = textError
      if (!this.phoneNumber) this.errorPhone = textError;
      // поля для заказа другому гостю

      if (!this.anotherCustomerPhone) this.errorAnotherPhone = textError;
      if (!this.anotherCustomerName) this.errorAnotherName = textError;
      // поля для адреса доставки
      if (!this.addressState.name) this.errorAddress = textError;
      // поле для предзаказа
      if (this.preOrderTime[0] === "Закрыто") this.errorPreOrderTime = preOrderTimeError;
      // if(!this.entrance) this.errorEntrance = textError
      // if(!this.floor) this.errorFloor = textError
      // if(!this.apartment) this.errorApartment = textError
      // поля для предзаказа
      // if(this.selectTime ===  '--- Выбрать ---') this.errorTime = textError
      // if(this.selectDay ===  '--- Выбрать ---') this.errorDay = textError
      // if(this.pointSelfDelivery ===  '--- Выбрать ---') this.errorPointSelfDelivery = textError

      //проверка полей в зависимости от типа доставки. Пример: если выбран самовывоз, проверка будет только групп phoneFieldErr и  pointSelfDelivery
      const phoneFieldErr = this.errorPhone ? this.errorPhone : this.errorPhoneAuth || this.errorName;
      const anotherPhoneFieldErr = Number(this.deliveryType) === 3 ? this.errorAnotherPhone || this.errorAnotherName : null;
      const addressFieldErr = !this.selectedAddress ? this.errorAddress : null;
      const preOrderTimeFieldErr = this.preOrderTime[0] === "Закрыто" ? this.errorPreOrderTime : null;
      // const pointSelfDelivery = Number(this.deliveryType === 2) ? this.errorPointSelfDelivery : null

      // console.log('noAuth = ', noAuth)
      // console.log('validationPhoneNumber = ', this.validationPhoneNumber)

      // console.log('phoneFieldErr', phoneFieldErr)
      // console.log('anotherPhoneFieldErr', anotherPhoneFieldErr)
      // console.log('addressFieldErr', addressFieldErr)
      // console.log('pointSelfDelivery', pointSelfDelivery)
      console.log("preOrderTimeFieldErr", preOrderTimeFieldErr, this.preOrderTime, this.preOrderTime[0], this.preOrderTime[0] === "Закрыто");

      if (phoneFieldErr || addressFieldErr || anotherPhoneFieldErr || preOrderTimeFieldErr) {
        return false;
      }

      return true;
    },

    checkBtn() {
      const textError = "Ошибка";
      const incorrectError = "Номер телефона не корректный";

      const incorrect = !this.validationPhoneNumber;

      this.errorPhoneAuth = incorrect ? incorrectError : null;
      if (!this.phoneNumber) this.errorPhone = textError;

      const phoneFieldErr = this.errorPhone ? this.errorPhone : this.errorPhoneAuth || this.errorName;

      return phoneFieldErr ? false : true;
    },

    clearError() {
      this.errorPhoneAuth = null;
      this.errorName = null;
      this.errorPhone = null;
      this.errorAnotherPhone = null;
      this.errorAnotherName = null;
      this.errorAddress = null;
      //  this.errorEntrance = null
      //  this.errorFloor = null
      //  this.errorApartment = null
      //  this.errorTime = null
      //  this.errorDay = null
      //  this.errorPointSelfDelivery = null
    },

    getNewAddressData() {
      const split = this.addressState.name.split(", ");
      const home = split.pop();
      const street = split.join(", ");
      const entrance = this.entrance ? `, ${this.entrance} подъезд` : "";
      const floor = this.floor ? `, ${this.floor} этаж` : "";
      const apartment = this.apartment ? ` , кв. ${this.apartment}` : "";

      const finish_addr = `${street}, д. ${home}${entrance}${floor}${apartment}`;

      // ? заменить полный адрес на this.fullAddr если исправят фикс
      return {
        customerId: this.customer.id,
        streetId: this.streetId,
        entrance: this.entrance,
        floor: this.floor,
        apartment: this.apartment,
        fullAddr: finish_addr,
        coord: this.addressState.coord,
        city: this.addressCity,
      };
    },

    saveNewAddress() {
      // ! как отправлять город?
      if (this.checkForm()) {
        this.hiddenAddBtn = true;
        const newAddressData = this.getNewAddressData();

        this.$store.dispatch("address/createAddress", newAddressData).then((res) => {
          this.selectedAddress = res;
          this.$store.state.address.id = res.id;
        });
        this.showForm = false;

        return;
      }
      return;
    },
    deleteAddress(address) {
      // if(!address.confirmed){
      //     this.commit('address/DELETE_ADDRESS', address)
      // } else{
      //     this.commit('address/DELETE_ADDRESS', address)
      //     this.$store.dispatch('address/deleteAddress', address.id)
      // }
      this.$store.dispatch("address/deleteAddress", address.id);
      this.$store.state.address.addresses = this.$store.state.address.addresses.filter((addr) => addr.id !== address.id);
      // if(!address.confirmed){
      //     console.log('address not confirmed', address)
      //     console.log('this.$store.state.address.addresses', this.$store.state.address.addresses)
      //     return this.$store.state.address.addresses = this.$store.state.address.addresses.filter(addr => addr.id !== address.id)
      // } else{
      //     console.log('address confirmed', address)
      // this.$store.state.address.addresses = this.$store.state.address.addresses.filter(addr => addr.id !== address.id)
      //     this.$store.dispatch('address/deleteAddress', address.id)
      // }
    },

    cancelAddress() {
      this.hiddenAddBtn = true;
      this.showForm = false;
      this.selectedAddress = this.getActualAddress;
    },

    addNewAddress() {
      this.hiddenAddBtn = false;
      this.errorAddress = null;
      this.selectedAddress = null;
      this.showForm = true;
    },

    selectAddress(address) {
      this.showForm = false;
      this.hiddenAddBtn = true;

      if (!address.actual) {
        this.$store.dispatch("address/setActualAddress", { customerId: this.customer.id, addressId: address.id });
        this.selectedAddress = address;
        this.addressState.name = address.street_id;
        this.addressState.coord = address.coord;
      } else {
        this.selectedAddress = address;
      }
      return;
    },

    logIn() {
      if (this.checkBtn()) {
        (this.errorPhoneAuth = null), (this.errorPhone = null), this.$store.commit("auth/setNumber", this.phoneNumber);
        this.$store.state.loginModal = 1;
        this.$store.state.loginProcess = true;
      }
      return;
    },

    checkOrder() {
      // проверить поля заказа перед отправкой
      // при успехе открыть форму ввода данных кредитной карты
      // console.log('this.noEmptyAddress', this.noEmptyAddress)
      // console.log('this.noEmptyNumber', this.noEmptyNumber)
      // ? !this.noEmptyAddress && !this.noEmptyNumber
      // console.log('checkValidFormPhone',this.checkValidFormPhone())
      // console.log('checkValidFormAddress',this.checkValidFormAddress())
      console.log("checkorder start", this.checkForm());

      if (this.checkForm())
        // !для теста пропускаем модалки по оплате и выходим сразу в createPay()
        // this.createPay({token: 'test', saveCard: false })

        // !раскоментировать после теста
        // if(this.saveCards.length) {
        //     this.openSaveCardModal()
        // } else {
        //     this.openAddCardModal()
        // }
        this.createPay({ token: "test", saveCard: false });

      return;
    },
    // showErrorCard(){
    //     console.log('test error')
    // },

    Pay(order) {
      this.$store.dispatch("payment/createPay", order).then((res) => {
        if (res.success && res.token) {
          const redirect = `${config.market_api_host}order/payment_confirmation_3ds/${config.devFlag}`;
          const orderId = this.orderId;
          const url = redirect + orderId;
          const token = res.token;
          // this.getPaymentConfirmation()
          this.openWidget({ token, url });
          // TODO закрыть модалку с оплатой, если магазин закроется

          // const pointEndWorkDelivery = this.workTime ? workTime.to : this.endWorkPoint
          // const EndHours = startHours > Number(pointEndWorkDelivery.split(':')[0]) ? Number(pointEndWorkDelivery.split(':')[0]) + 24 : Number(pointEndWorkDelivery.split(':')[0])
          // const EndMinutes = Number(pointEndWorkDelivery.split(':')[1])

          // const timeToClose = 10000
          // this.watchPayment = setInterval(() => {
          //     this.closeModal()
          // },timeToClose)
        } else if (res.success && res.redirect) {
          this.urlRedirect = res.redirect.url;
          this.getPaymentConfirmation();
          this.openConfirmPayModal();
        } else if (res.success) {
          // почистить локал от оплаченного заказа

          return this.$router.push({ name: "deliveryOrder", params: { id: this.orderId } });
        } else if (!res.success) {
          this.showErrorCard();
        }
      });
    },
    //формируем данные для отправки на оплату
    createPay(paymentData) {
      // ! добавить обновление юзера

      //Номер телефона и имя если есть
      // ?возможны проблемы с форматом номера
      const name = this.customer.name ? this.customer.name + " " : "";
      const phone = this.customer.number;
      const customerInfo = `${name}${phone}`;

      const customerName = this.customerName;
      if (customerName) {
        this.customer.name = customerName;
        this.rememberCustomerName({ customer_number: phone, customer_name: customerName })
          .then((res) => console.log(res))
          .catch((err) => console.log(err));
      }

      //формирование имени заказа
      const nameDelivery = `маркетплейс ${this.orderId}`;

      // доставка другому
      const anotherCustomerInfo = this.deliveryType == 3 ? `доставка другому гостю. Имя гостя: ${this.anotherCustomerName}, номер телефона: ${this.anotherCustomerPhone}. ` : "";

      let point_id;
      // адрес точки
      let start_addr = "";
      console.log("this.getPoint", this.brandPoints);

      if (this.brandPoints.length > 1) {
        point_id = Number(this.deliveryType) !== 2 ? null : this.pointSelfDelivery.delivery_id;
        start_addr = Number(this.deliveryType) !== 2 ? null : this.pointSelfDelivery.address;
        console.log("this.deliveryType", this.deliveryType);
        console.log("test > 1");
      } else if (this.brandPoints.length === 1) {
        point_id = Number(this.deliveryType) !== 2 ? this.getPoint[0].pointId : this.pointSelfDelivery.delivery_id;
        start_addr = Number(this.deliveryType) !== 2 ? this.getPoint[0].addressPoint : this.pointSelfDelivery.address;
        console.log("this.deliveryType", this.deliveryType);
        console.log("test === 1");
      } else {
        // * на будущее для заказа с нескольких мест
        // * будет массив точек и массив адресов
        // this.getPoint.forEach(point => {
        //     point_id.push(point.pointId)
        //     start_addr.push(point.addressPoint)
        // })
        throw new Error("Нету точек");
      }

      // формирование адреса доставки для создания и finish_addr для заказа
      let addrObj = {};
      // если адрес сохранен, просто передаем id
      if (this.selectedAddress && Number(this.deliveryType) !== 3) {
        addrObj.addressId = this.selectedAddress.id;
      } else {
        // const split = this.addressState.name.split(', ')
        // const home = split.pop()
        // const street = split.join(', ')
        // const entrance = this.entrance? `, ${this.entrance} подъезд` : ''
        // const floor = this.floor? `, ${this.floor} этаж`: ''
        // const apartment = this.apartment? ` , кв. ${this.apartment}`: ''

        addrObj.street = this.getNewAddressData().streetId;
        addrObj.entrance = this.getNewAddressData().entrance;
        addrObj.floor = this.getNewAddressData().floor;
        addrObj.apartment = this.getNewAddressData().apartment;
        addrObj.finish_addr = this.getNewAddressData().fullAddr;
      }

      const deliveryMode = Number(this.deliveryMode) === 1 ? "🔥 Ближайший" : "⏰ Предзаказ";
      // самовывоз
      // if(this.deliveryType === 2){
      // точка забора заказа
      //     const date = this.selectDay
      //     const time = this.selectTime
      //     const test = time.split(':')

      //     const y = date.getFullYear()
      //     const m = date.getMonth()
      //     const d = date.getDay()
      //     const newTestDate = new Date(y, m, d, Number(test[0]), Number(test[1]));
      //     console.log('newTestDate = ',newTestDate.toISOString());

      //     const selfDeliveryInfo = {
      //     name: nameDelivery,
      //     guest: customerInfo,
      //     source: 4,
      //     point_id:245, //* для теста (pointSelfDelivery.id)
      //     start_addr: pointSelfDelivery.address,
      //     mode: deliveryMode,
      //     descr: this.selectedAddress ? this.selectedAddress.comment : this.comment,
      //     coordinates: this.addressState.coord,
      //     ...addrObj
      //     }

      // }

      const deliveryData = {
        name: nameDelivery,
        comment: this.comment,
        guest: customerInfo,
        source: 4,
        // если точек больше 1 у бренда, сначала отправляем логисту на почту
        point_id, //* для теста
        type: this.deliveryType,
        mode: deliveryMode,
        preOrderDate: this.preOrderDate,
        descr: anotherCustomerInfo + this.comment,
        coordinates: this.addressState.coord,
        start_addr,
        ...addrObj,
      };

      console.log("deliveryData", deliveryData);

      let card;

      if (paymentData.token === "test") {
        card = {
          paymentToken: paymentData.token,
          saveCard: paymentData.saveCard,
        };
      } else if (paymentData.cardId) {
        card = {
          cardId: paymentData.cardId,
        };
      } else {
        card = {
          paymentToken: paymentData.token,
          saveCard: paymentData.saveCard,
        };
      }

      const order = {
        amount: {
          value: Number(this.deliveryType) === 2 ? this.getAmountCart : this.totalAmount,
          currency: this.currency,
        },
        orderId: this.orderId,
        customerId: this.customer.id,
        widget: true,
        ...card,
        ...deliveryData,
        now: new Date().toISOString(),
      };
      //TODO проверка на время работы?

      //сохраняем данные в local

      this.Pay(order);
      // localStorage.setItem("orderPay", JSON.stringify(order))
    },

    next(data) {
      if (data.addCard) {
        this.openAddCardModal();
      } else {
        data.cardId;
        this.createPay({
          cardId: data.cardId,
        });
      }
    },

    getPaymentConfirmation() {
      this.$store.dispatch("payment/getPaymentConfirmation", this.orderId).then((res) => {
        if (res.success) {
          this.showModal = this.SHOW_MODAL.hide;
          return this.$router.push({ name: "deliveryOrder", params: { id: this.orderId } });
        }
      });
    },

    sendToHome() {
      this.$store.state.selectedBrand = null;
      return this.$router.push("/");
    },

    sendToBrand() {
      return this.$router.push("home");
    },

    closeModal() {
      this.showModal = this.SHOW_MODAL.hide;
      this.$store.commit("order/CLOSE_WIDGET");
    },

    openConfirmPayModal() {
      return (this.showModal = this.SHOW_MODAL.confirmPayModal);
    },

    openAddCardModal() {
      return (this.showModal = this.SHOW_MODAL.addCardModal);
    },
    openWidget(data) {
      const confirmationToken = data.token;
      const returnUrl = data.url;
      this.$store.commit("order/SET_WIDGET", { returnUrl, confirmationToken });
      this.$store.commit("order/SHOW_WIDGET");
    },

    openSaveCardModal() {
      return (this.showModal = this.SHOW_MODAL.saveCardsModal);
    },
    openWidgetModal() {
      return (this.showModal = this.SHOW_MODAL.widget);
    },

    closeLogin() {
      this.$store.state.loginProcess = false;
    },

    // async validationPhoneNumber() {
    //     let formData = {
    //         number: this.phoneNumber,
    //         checked: 1,
    //     };
    //     this.valid = await AuthSchema.isValid(formData);
    // },

    PlusCountDish(item) {
      item.count++;
      // полная стоимость позиции (не факт что нужно)
      item.totalPrice = item.price * item.count;
      return;
    },

    MinusCountDish(item) {
      item.count--;
      if (item.count === 0) {
        // лучше вставить предупреждение об удалении из корзины
        return (this.$store.state.order.cartItems = this.$store.state.order.cartItems.filter((order) => order.id !== item.id));
      }
      item.price = item.price * item.count;
      return;
    },

    deleteTrash() {
      return (this.$store.state.order.cartItems = []);
    },

    // обработка нажатия на Подробнее у блюда в корзине
    addsDishMore(dish) {
      let dishCart = this.$store.state.order.cartItems.find((el) => el == dish);
      if (dishCart) {
        let index = this.$store.state.order.cartItems.indexOf(dishCart);
        if (index != -1) this.$store.state.order.cartItems.splice(index, 1);
      }
      dish.openMore = true;
      this.$store.state.order.cartItems.unshift(dish);
      this.$store.commit("order/SHOW_DISH_MODAL");
    },
    plusCountAddToOrder(dish) {
      console.log("plusCountAddToOrder");
      this.$store.state.order.cartItems.unshift(dish);
      // удалить из добавления к заказу
      this.$store.commit("order/DELETE_ADD_TO_ORDER", dish);
      this.$store.dispatch("order/updateCart");
    },
    clickMoreDetails(dish) {
      let dishOrd = this.newDishForCart(dish);
      this.$store.state.order.cartItems.unshift(dishOrd);
      this.$store.commit("order/SHOW_DISH_MODAL");
    },
    newDishForCart(dish) {
      let adds_dish = [];
      for (let elem of dish.adds_dish) {
        adds_dish.push({
          count: 0,
          delete: elem.delete,
          description: elem.description,
          dish_id: elem.dish_id,
          id: elem.id,
          index: elem.index,
          name: elem.name,
          picture: elem.picture,
          price: elem.price,
          status: elem.status,
          uploaded: true,
          weight: elem.weight,
        });
      }
      return {
        adds_dish: adds_dish,
        count: 1,
        create_date: dish.create_date,
        delete: dish.delete,
        description: dish.description,
        dish_favorite: dish.dish_favorite,
        favorite_counter: dish.favorite_counter,
        id: dish.id,
        images: dish.images,
        index: dish.index,
        name: dish.name,
        picture: dish.picture,
        brand_id: dish.brand_id,
        price: dish.price,
        status: dish.status,
        weight: dish.weight,
        settings: dish.settings,
      };
    },
    // Проверка блюда, является ли он понравившимся
    checkFavorite(dish_id) {
      return this.favorite_dishes.find((el) => el.dishId == dish_id);
    },
    // Обработка клика по сердцу у блюда
    clickFavorite(dish_id) {
      let item = {
        dishId: dish_id,
        customerId: this.customer.id,
      };
      if (this.checkFavorite(dish_id)) {
        let elem = this.checkFavorite(dish_id);
        let ind = this.favorite_dishes.indexOf(elem);
        this.$store.commit("order/DELETE_FAVORITE_DISHES", ind);
        this.$store.dispatch("deleteFavoriteDishes", item);
      } else {
        this.$store.commit("order/ADD_FAVORITE_DISHES", item);
        this.$store.dispatch("postFavoriteDishes", item);
      }
    },
    getPortionType(type) {
      type = Number(type);
      let str = "";
      if (type == 0 || type == "") str = " г";
      if (type == 1) str = " кг";
      if (type == 2) str = " л";
      if (type == 3) str = " мл";
      if (type == 4) str = " шт";
      return str;
    },
  },
};
</script>
<style lang="scss" scope>
@mixin fontStyle($font-style, $font-weight, $font-size, $line-height) {
  font-family: Rubik;
  font-style: $font-style;
  font-weight: $font-weight;
  font-size: $font-size !important;
  line-height: $line-height;
  margin-bottom: 0px !important;
}

.b1 {
  @include fontStyle(normal, 500, 16px, 150%);

  @media screen and (max-width: 1599px) {
    @include fontStyle(normal, 500, 14px, 140%);
  }
  &-opacity {
    opacity: 0.5;
  }
  &-normal {
    font-weight: normal;
  }
}
.b2 {
  @include fontStyle(normal, 500, 14px, 140%);

  @media screen and (max-width: 1599px) {
    @include fontStyle(normal, 500, 12px, 135%);
  }
  &-opacity {
    opacity: 0.5;
  }
  &-normal {
    font-weight: normal;
  }
}

.textUP {
  @include fontStyle(normal, 500, 18px, 24px);
  &--wrapper {
    margin-top: 1.875rem;
    margin-bottom: 1.25rem;

    &:first-child {
      margin-top: 0;
    }
    @media screen and (max-width: 799px) {
      margin-bottom: 1rem;
    }
  }

  @media screen and (max-width: 1599px) {
    @include fontStyle(normal, 500, 16px, 20px);
    margin-bottom: 1rem !important;
  }
  &-opacity {
    opacity: 0.5;
  }
  &-normal {
    font-weight: normal;
  }
}
.cart-btn {
  &--footer {
    &-wrapper {
      padding: 10px 30px 30px;
      position: relative;
      z-index: 1;
    }

    &-info {
      display: flex;
      justify-content: space-between;
    }
    &-pay {
      &-wrapper {
        margin-top: 30px;
        display: none;

        @media screen and (max-width: 1279px) {
          margin-top: 30px;
          display: block;
        }
      }
      &-btn {
        cursor: pointer;
        display: inline-block;
        text-align: center;
        transition: 0.2s ease-out;
        transition-property: background-color, color, border-color;
        color: #0c043f;
        width: 100%;
        padding: 15px 20px;
        background: linear-gradient(49.06deg, #f9c801 20.38%, #fff854 135.95%);
        border-radius: 10px;
        border: none;
        font-weight: 500;
      }
      &-btn:hover {
        background: linear-gradient(49.06deg, #ffb320 20.38%, #fff854 135.95%);
      }
      &-btn:active {
        opacity: 0.8;
        transform: translateY(2px);
      }
    }
  }
}

.wrapper-main {
  max-width: 1530px;
  margin-left: auto;
  margin-right: auto;
  margin-top: 0px;
  display: flex;
  position: relative;

  @media screen and (min-width: 375px) {
    width: calc(100% - 30px);
  }
  @media screen and (min-width: 414px) {
    // max-width: 384px;
    width: calc(100% - 30px);
  }
  @media screen and (min-width: 800px) {
    // max-width: 760px;
    width: calc(100% - 40px);
  }
  @media screen and (min-width: 1280px) {
    max-width: 1170px;
    width: calc(100% - 90px);
  }
  @media screen and (min-width: 1600px) {
    max-width: 1530px;
    width: calc(100% - 70px);
  }
}
.main-order {
  flex: 0 0 66.666667%;
  max-width: 66.666667%;
  margin-top: 30px;
  margin-right: 30px;

  @media screen and (max-width: 1279px) {
    flex: 0 0 100%;
    max-width: 100%;
    //padding: 0 40px;
    margin-right: 0;
  }
  @media screen and (max-width: 799px) {
    flex: 0 0 100%;
    max-width: 100%;
    padding: 0;
  }
  // .breadcrumbs{
  //     display:flex;
  //     width: min(320px, 80%);
  //     &-position{
  //         display: flex;
  //         align-items: baseline;
  //         width: min(320px, 80%);
  //     }
  //     &-item{
  //         padding:0 7px;
  //         &:hover{
  //             opacity:1;
  //             cursor: pointer;
  //         }
  //         &:first-child{
  //             padding-left:0;
  //         }
  //         &:hover:last-child{
  //             opacity:0.5;
  //             cursor: default;
  //         }
  //     }
  // }
  .title {
    margin-top: 17px;

    @media screen and (max-width: 799px) {
      margin-top: 11px;
    }
  }

  &--body {
    margin-top: 30px;
    @media screen and (max-width: 799px) {
      margin-top: 20px;
    }

    .order {
      background: #ffffff;
      /* 1 */
      box-shadow: 0px 0px 50px rgba(9, 10, 19, 0.08);
      border-radius: 30px;
      &--min-height {
        min-height: 150px;
      }

      &-block {
        margin-top: 30px;

        &--radio {
          margin-top: 15px;
          @media screen and (max-width: 799px) {
            margin-top: 5px;
          }
        }

        &:first-child {
          margin-top: 0px;
        }
        &--element-wrapper {
          margin-top: 10px;

          // @media screen and (max-width: 799px){
          //     margin-top: 5px;
          // }
        }
      }

      &--wrapper {
        padding: 30px;

        @media screen and (max-width: 1599px) {
          padding: 30px 23px;
        }
        @media screen and (max-width: 799px) {
          padding: 30px 10px;
        }

        &-form {
          padding: 0;

          @media screen and (max-width: 799px) {
            padding: 0 5px;
          }
        }
      }
    }
    .form-customer {
      justify-content: flex-start !important;
      display: flex;
      margin-top: 10px;
      // margin-top: 20px;

      @media screen and (max-width: 799px) {
        flex-direction: column;
      }
      &--last {
        margin-right: 0 !important;
        margin-bottom: 0 !important;
      }
      &--options {
        margin-top: 0;
        &-wrapper {
          @media screen and (max-width: 799px) {
            flex-direction: row;
          }
        }
      }

      &--input {
        // flex: 0 0 50%;
        width: 100%;
        max-width: min(350px, 100%);
        margin-right: 16px;

        @media screen and (max-width: 799px) {
          margin-bottom: 10px;
          margin-right: 0;
        }
        // @media screen and (max-width: 549px){
        // }
        &-street {
          max-width: min(470px, 100%);
        }
        &-select {
          @media screen and (max-width: 799px) {
            margin-right: 10px;
          }

          &-date {
            max-width: min(226px, 100%);
          }
          &-time {
            max-width: min(160px, 100%);
          }
          &-self {
            max-width: min(580px, 100%);
          }
        }
        &-options {
          max-width: min(230px, 100%);
          @media screen and (max-width: 799px) {
            margin-right: 10px;
          }
        }
        &-comment {
          max-width: min(668px, 100%);
        }
        &-order {
          @media screen and (max-width: 799px) {
            order: -1;
          }
        }
      }

      &--block-confirm {
        width: min(209px, 100%);

        @media screen and (max-width: 799px) {
          margin-top: 5px;
          // margin-bottom:30px;
        }
      }
      &--block-name {
        display: flex;
        flex-basis: 100%;

        @media screen and (max-width: 549px) {
          flex-direction: column;
          align-items: flex-start;
        }
      }
      &--name {
        width: min(350px, 100%);
        margin-right: 16px;

        @media screen and (max-width: 799px) {
          margin-bottom: 10px;
          margin-right: 0;
        }
      }
      &--form-btn {
        flex: 0 0 25%;
        max-width: 25%;
      }
      &--edit {
        min-width: min(154px, 100%);
        width: 154px;
        &:active {
          transform: translateY(2px);
          opacity: 0.8;
        }
      }
      &--btn-edit {
        border: none !important;
        padding: 0 !important;
      }
      &--confirm {
        margin-top: 17px;
        @media screen and (max-width: 520px) {
          display: none;
        }
      }
    }
    .address-wrapper {
      margin-top: 10px;
      &:last-child {
        @media screen and (max-width: 799px) {
          margin-top: 0;
        }
      }
    }
  }

  &--footer {
  }
}

.wrapper-content {
  @media screen and (max-width: 799px) {
    padding: 0 10px;
  }
}

.address-item {
  display: flex;
  padding: 10px;
  background: #f7f7f7;
  border-radius: 10px;
  box-sizing: border-box;
  margin: 0 16px 10px 0 !important;
  min-height: 44px;
  cursor: pointer;
  width: 100%;
  max-width: 424px;
  @media screen and (max-width: 1599px) {
    max-width: 350px;
  }

  &--wrapper {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;

    @media screen and (max-width: 799px) {
      flex-direction: column;
    }
  }

  &--check {
    margin-right: 15px;
  }
  &--text {
    display: flex;
    width: 100%;
    justify-content: space-between;
  }
  &--delete-icon {
    border-radius: 10px;
    display: flex;
    align-items: center;
    opacity: 0.5;
    padding: 0 5px 0 15px;
    border: 0px;
    background: #fff0;

    &:hover {
      opacity: 1;
    }
    &:active {
      transform: scale(1.1);
    }
    @media (pointer: coarse) {
      &:active {
        opacity: 1;
        transform: translateY(2px);
      }
    }
  }
  &:hover {
    .address-item--close-icon {
      opacity: 1;
      transition: opacity 0.5s;
    }
  }
  &--close-icon {
    opacity: 0;
    transform: opacity 2s;

    @media (pointer: coarse) {
      opacity: 1 !important;
    }
  }

  & > span {
    text-transform: capitalize;
  }
}
.form-check.form-check-inline {
  margin-top: 15px !important;
}
.selectAddress {
  background: rgba(33, 199, 119, 0.1);
  cursor: default;
  span {
    color: rgba(33, 199, 119, 1);
  }
}
.trash-position {
  position: relative;
  flex: 0 0 400px;
  margin-left: 30px;
}
.trash {
  display: flex;
  margin-top: 0px;
  width: 400px;
  top: 0px;
}
.content {
  width: 100%;
  height: auto;
}
.btn-style {
  background: #7a2a9a !important;
  border-radius: 10px !important;
  width: 209px;
  padding: 11px 0px !important;
  border: none;
  &:hover {
    background: #7b1dc5 !important;
  }
  &:active {
    transform: translateY(2px);
    opacity: 0.8;
  }
}
.disabledBtn {
  cursor: help !important;
  opacity: 0.5;
}
.btn-newAddress {
  width: 118px;
  height: 48px;
  align-items: center;
  border: 0px;
}
.btn-newAddress-cancel {
  background: #eaebee !important;
  width: 92px;
  color: #0c043f;
  &:hover {
    background: #eaebee !important;
  }
  &:active {
    transform: translateY(2px);
    opacity: 0.8;
  }
}
.style-input {
  border-radius: 10px !important;
  color: #031e4d !important;
  height: 48px !important;
  border: 1px solid rgba(3, 30, 77, 0.1) !important;
}
input.style-input:focus {
  animation: shadowAnimation 2s infinite cubic-bezier(0.4, 0, 1, 1);
  animation-direction: alternate-reverse;
}
select.style-input:focus {
  animation: shadowAnimation 2s infinite cubic-bezier(0.4, 0, 1, 1);
  animation-direction: alternate-reverse;
}
/*оформление ошибки для select. ошибка не пишется. просто выделяется красным цветом*/
.style-select_error {
  animation: opacityAnimation 2s infinite cubic-bezier(0.4, 0, 1, 1);
  border: none !important;
  // background: rgba(245, 44, 44, 0.08)!important;
  // animation-direction: alternate-reverse;
}
.animation-enter-active {
  transition: all 0.8s ease;
}
.animation-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.animation-enter-from, .animation-leave-to /* .auth_error-leave-active до версии 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
.animationReverse-enter-active {
  transition: all 0.3s ease;
}
.animationReverse-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}
.animationReverse-enter-from, .animationReverse-leave-to /* .auth_error-leave-active до версии 2.1.8 */ {
  transform: translateX(-10px);
  opacity: 0;
}
/*оформление ошибки input для телефона */
.style-input-auth_error {
  border-radius: 5px;
  padding: 10px;
  margin-top: 8px;
  border: 1px solid rgba(245, 44, 44, 0.8) !important;
  color: white;
  background: rgba(245, 44, 44, 0.8);
  // animation-direction: alternate-reverse;
  // animation: opacityAnimation 2s infinite cubic-bezier(0.4, 0, 1, 1);
}
/*оформление ошибки для input*/
.style-input_error {
  animation: opacityAnimation 2s infinite cubic-bezier(0.4, 0, 1, 1);
  border: none !important;
  background: rgba(245, 44, 44, 0.08) !important;
  animation-direction: alternate-reverse;
}
/*оформление ошибки для input (ошибка пишется в placeholder)*/
.style-input_error::placeholder {
  color: #0c043f !important;
  font-size: 16px;
}
.card-elem-address {
  background: #f7f7f7;
  border-radius: 10px;
  box-sizing: border-box;
  // margin: 10px 0px !important; /*2px*/
  // height: 44px;
  // font-family: Rubik;
  // font-style: normal;
  // font-weight: 500;
  // font-size: 18px;
  // line-height: 24px;
  cursor: pointer;
}
/* для элемента input c type="radio" */
.custom-radio {
  position: absolute !important;
  z-index: -1;
  opacity: 0;
}
/* для элемента label связанного с .custom-radio */
.custom-radio + label {
  display: inline-flex;
  align-items: center;
  user-select: none;
  color: rgba(12, 4, 63, 0.5);
}
/* создание в label псевдоэлемента  before со следующими стилями */
.custom-radio + label::before {
  content: "";
  display: inline-block;
  width: 1em;
  height: 1em;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid #7a2a9a;
  border-radius: 50%;
  margin-right: 0.5em;
  background-position: center center;
  /*background-size: 120% 120%;*/
}
/* стили при наведении курсора на радио */
.custom-radio:not(:disabled):not(:checked) + label:hover::before {
  border-color: #a776bb;
}
/* стили для активной радиокнопки (при нажатии на неё) */
.custom-radio:not(:disabled):active + label::before {
  background-color: #7a2a9a;
  border-color: #7a2a9a;
}
/* стили для радиокнопки, находящейся в фокусе */
.custom-radio:focus + label::before {
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}
/* стили для радиокнопки, находящейся в состоянии checked */
.custom-radio:checked + label::before {
  background-image: url("/assets/icon/Radiobutton.svg");
}
.custom-radio:checked + label {
  color: #7a2a9a;
}
/* стили для радиокнопки, находящейся в состоянии disabled*/
.custom-radio:disabled + label::before {
  opacity: 0.5;
}
.custom-radio:disabled + label {
  opacity: 0.5;
  color: #9a2a2a !important;
}

@keyframes opacityAnimation {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0.8;
  }
  100% {
    opacity: 0.4;
  }
}
@keyframes showAnimation {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 0.5;
  }
  100% {
    opacity: 0.1;
  }
}
@keyframes shadowAnimation {
  0% {
    box-shadow: 0 0 0 0.05rem rgb(98, 98, 98);
  }
  50% {
    box-shadow: 0 0 0 0.05rem rgba(98, 98, 98, 0.82);
  }
  100% {
    box-shadow: 0 0 0 0.05rem rgba(98, 98, 98, 0.211);
  }
}

input,
textarea {
  outline: none;
}
input:active,
textarea:active {
  outline: none;
}
:focus {
  outline: none;
}

/* custom select*/

.select-wrapper {
  position: relative;
  user-select: none;
  width: 100%;
}
.select {
  position: relative;
  display: flex;
  flex-direction: column;
  border-width: 0 2px 0 2px;
  border-style: solid;
  border-color: #394a6d;
}
.select__trigger {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 22px;
  font-size: 20px;
  font-weight: 300;
  color: #3b3b3b;
  height: 60px;
  line-height: 60px;
  background: #ffffff;
  cursor: pointer;
  border-width: 2px 0 2px 0;
  border-style: solid;
  border-color: #394a6d;
}
.custom-options {
  position: absolute;
  display: block;
  top: 100%;
  left: 0;
  right: 0;
  border: 2px solid #394a6d;
  border-top: 0;
  background: #fff;
  transition: all 0.5s;
  visibility: hidden;
  pointer-events: none;
  z-index: 2;
}
.select.open .custom-options {
  opacity: 1;
  visibility: visible;
  pointer-events: all;
}
.custom-option {
  position: relative;
  display: block;
  padding: 0 22px 0 22px;
  font-size: 22px;
  font-weight: 300;
  color: #3b3b3b;
  line-height: 60px;
  cursor: pointer;
  transition: all 0.5s;
}
.custom-option:hover {
  cursor: pointer;
  background-color: #b2b2b2;
}
.custom-option.selected {
  color: #ffffff;
  background-color: #305c91;
}

/*suctom select arrow*/
.arrow {
  position: relative;
  height: 15px;
  width: 15px;
}
.arrow::before,
.arrow::after {
  content: "";
  position: absolute;
  bottom: 0px;
  width: 0.15rem;
  height: 100%;
  transition: all 0.5s;
}
.arrow::before {
  left: -5px;
  transform: rotate(45deg);
  background-color: #394a6d;
}
.arrow::after {
  left: 5px;
  transform: rotate(-45deg);
  background-color: #394a6d;
}
.open .arrow::before {
  left: -5px;
  transform: rotate(-45deg);
}
.open .arrow::after {
  left: 5px;
  transform: rotate(45deg);
}

.order-address-street--input {
  width: 100%;
  height: 100%;
  cursor: pointer;
}
.order-address-street--input:read-only {
  background: none !important;
}
.cart-btn--footer-pay-btn:disabled {
  opacity: 0.5;
  background: linear-gradient(49.06deg, #f9c801 20.38%, #fff854 135.95%);
  transform: none;
  cursor: default;
}

/* =======================================INPUT-LOADER======================================= */

.loader {
  // display: inline-block;
  // position: absolute;
  // top: 3px;
  width: 20px;
  height: 20px;
  margin-right: 20px;
  margin-bottom: 15px;

  &--position {
    width: 100%;
    height: 100%;
    display: flex;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    justify-content: center;
    align-items: center;
    background: rgba(255, 255, 255, 0.4);
    z-index: 9999;
  }
}

.loader > div {
  transform-origin: 20px 20px;
  border-radius: 5px;
}

.loader > div:after {
  animation: run-spinner 1.6s linear infinite;
}

.loader > div:after {
  content: "";
  display: block;
  position: absolute;
  width: 3px;
  height: 6px;
  top: 8px;
  left: 18px;
  background: #0c043f;
  border-radius: 5px;
}
.loader > div:before {
  content: "";
  display: block;
  position: absolute;
  width: 3px;
  height: 6px;
  top: 8px;
  left: 18px;
  /* background: #F9C801; */
  border-radius: 5px;
}

.loader > *:nth-child(1) {
  transform: rotate(0deg);
}
.loader > *:nth-child(1):after {
  animation-delay: 0s;
}

.loader > *:nth-child(2) {
  transform: rotate(45deg);
}
.loader > *:nth-child(2):after {
  animation-delay: 0.2s;
}

.loader > *:nth-child(3) {
  transform: rotate(90deg);
}
.loader > *:nth-child(3):after {
  animation-delay: 0.4s;
}

.loader > *:nth-child(4) {
  transform: rotate(135deg);
}
.loader > *:nth-child(4):after {
  animation-delay: 0.6s;
}

.loader > *:nth-child(5) {
  transform: rotate(180deg);
}
.loader > *:nth-child(5):after {
  animation-delay: 0.8s;
}

.loader > *:nth-child(6) {
  transform: rotate(225deg);
}
.loader > *:nth-child(6):after {
  animation-delay: 1s;
}

.loader > *:nth-child(7) {
  transform: rotate(270deg);
}
.loader > *:nth-child(7):after {
  animation-delay: 1.2s;
}

.loader > *:nth-child(8) {
  transform: rotate(315deg);
}
.loader > *:nth-child(8):after {
  animation-delay: 1.4s;
}

@keyframes run-spinner {
  0% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>
