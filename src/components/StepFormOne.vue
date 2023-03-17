<script>
import { getCountries } from '../service/CountriesService.js';

import ModalForm from './ModalForm.vue';


export default {
    name: "StepFormOne",
    components: {
        ModalForm,


    },
    data() {
        return {
            activeForm: 1,
            activeOne: true,
            activeTwo: false,
            activethree: false,
            countries: [],
            selectedCountry: null,
            showModal: false,
            showModalValidateOne: false,
            showModalValidateTwo: false,
            showModalValidateThree: false,
            dataTitle: '',
            dataSection: '',
            data: {
                country: 'Colombia',
                gender: '',
                firstName: '',
                lastName: '',
                birthDate: '',
                documentType: 'Cédula de ciudadanía',
                documentNumber: 0,
                fileUpload: null,
                fileUploadBack: null,
                emailAddress: '',
                password: '',
                repeatPassword: '',
                phone: 0,
                phoneMovil: 0,
                streetAddress: '',
                postalCode: ''
            }
        };
    },
    created() {
        getCountries()
            .then(data => {
                this.countries = data;
            });
    },

    methods: {
        uploadDocumen(event) {
            this.data.fileUpload = event.target.files[0]
            console.log(event.target.files[0]);
        },
        uploadDocumenBack(event) {
            this.data.fileUploadBack = event.target.files[0]
            console.log(event.target.files[0]);
        },
        process() {
            if (!this.data.streetAddress || !this.data.postalCode) {
                    this.showModalValidateThree = true;
                    this.dataTitle = '¡Datos incorrectos!'
                    this.dataSection = '¡Recurde que todos los campos son requeridos.'
                    this.showModal = false;
                    return false
                };
            this.showModal = true;
            this.dataTitle = '¡Éxito en el envío de datos!'
            this.dataSection = '¡Felicidades! Tu registro ha sido exitoso y tus datos personales han sido guardados de manera segura.'
            console.log(this.data);
        },
        gotoStep(step) {
            if (step == 2) {
                if (!this.data.country || !this.data.gender || !this.data.firstName
                    || !this.data.firstName || !this.data.lastName || !this.data.birthDate
                    || !this.data.documentType || !this.data.documentNumber || !this.data.fileUpload
                    || !this.data.fileUploadBack) {
                    this.showModalValidateOne = true;
                    this.dataTitle = '¡Datos incorrectos!'
                    this.dataSection = '¡Recurde que todos los campos son requeridos.'

                    return false
                };
                this.activeTwo = true;
            };
            if (step == 3){
                if (!this.data.emailAddress || !this.data.password || !this.data.repeatPassword
                    || !this.data.phone || !this.data.lastName || !this.data.phoneMovil) {
                    this.showModalValidateTwo = true;
                    this.dataTitle = '¡Datos incorrectos!'
                    this.dataSection = '¡Recurde que todos los campos son requeridos.'

                    return false
                };
                this.activethree = true;
            };
            
            this.activeForm = step;


        }
    },
};
</script>

<template >
    <div class="max-w-4xl mx-auto my-5">
        <div
            class="relative after:absolute after:inset-x-0 after:top-1/2 after:block after:h-0.5 after:-translate-y-1/2 after:rounded-lg after:bg-gray-100">
            <ol class="relative z-10 flex justify-between text-sm font-medium text-gray-500">
                <li class="flex items-center gap-2 bg-white p-2">
                    <span class="h-6 w-6 rounded-full bg-gray-100 text-center text-[10px] font-bold leading-6 "
                        :class="{ 'bg-indigo-600 text-white': activeOne }">
                        1
                    </span>

                    <span class="hidden sm:block"> Datos </span>
                </li>

                <li class="flex items-center gap-2 bg-white p-2">
                    <span class="h-6 w-6 rounded-full bg-gray-100 text-center text-[10px] font-bold leading-6 "
                        :class="{ 'bg-indigo-600 text-white': activeTwo }">
                        2
                    </span>

                    <span class="hidden sm:block"> Seguridad </span>
                </li>

                <li class="flex items-center gap-2 bg-white p-2">
                    <span class="h-6 w-6 rounded-full bg-gray-100 text-center text-[10px] font-bold leading-6"
                        :class="{ 'bg-indigo-600 text-white': activethree }">
                        3
                    </span>

                    <span class="hidden sm:block"> Ubicación </span>
                </li>
            </ol>
        </div>

        <form ref="myForm" v-on:submit.prevent="process()">
            <div class="mt-10" v-if="activeForm == 1">
                <!-- Formulario 1 -->
                <div class="overflow-hidden drop-shadow-lg sm:rounded-xl">
                    <div class="bg-white px-4 py-5 sm:p-6">
                        <div class="grid grid-cols-6 gap-6">
                            <div class="col-span-6 sm:col-span-3">
                                <label for="country" class="block text-sm font-medium leading-6 text-gray-900">Pais</label>
                                <select id="country" name="country" v-model="data.country" @change="printSelectedOption"
                                    class="mt-2 block w-full h-9 rounded-md border-0 bg-white py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                                    <option v-for="country in countries">{{ country.name.common }}</option>
                                </select>
                            </div>
                            <div class="col-span-6 sm:col-span-3">
                                <label for="gender" class="block text-sm font-medium leading-6 text-gray-900">
                                    Genero
                                </label>
                                <input type="text" required="required" name="gender" id="gender" autocomplete="sex"
                                    class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    v-model="data.gender" />
                            </div>
                            <div class="col-span-6 sm:col-span-3">
                                <label for="first-name" class="block text-sm font-medium leading-6 text-gray-900">Primer
                                    Nombre</label>
                                <input type="text" required="required" name="first-name" id="first-name"
                                    autocomplete="given-name"
                                    class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    v-model="data.firstName" />

                            </div>

                            <div class="col-span-6 sm:col-span-3">
                                <label for="last-name" class="block text-sm font-medium leading-6 text-gray-900">Segundo
                                    Nombre</label>
                                <input type="text" required="required" name="last-name" id="last-name"
                                    autocomplete="family-name"
                                    class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    v-model="data.lastName" />
                            </div>
                            <div class="col-span-6 sm:col-span-4">
                                <label for="birth-date" class="block text-sm font-medium leading-6 text-gray-900">Fecha
                                    de nacimiento</label>
                                <input type="date" required="required" name="birth-date" id="birth-date"
                                    class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                    v-model="data.birthDate" />
                            </div>

                            <div class="col-span-6 sm:col-span-3">
                                <label for="document-type" class="block text-sm font-medium leading-6 text-gray-900">Tipo de
                                    documento</label>
                                <select id="document-type" name="document-type" v-model="data.documentType"
                                    class="mt-2 block w-full h-9 rounded-md border-0 bg-white py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6">
                                    <option>Cédula de ciudadanía</option>
                                    <option>Pasaporte</option>
                                    <option>Cédula de extranjería</option>
                                </select>
                            </div>

                            <div class="col-span-6 sm:col-span-3">
                                <label for="document-number"
                                    class="block text-sm font-medium leading-6 text-gray-900">Numero de documento
                                </label>
                                <input type="number" required="required" name="document-number" id="document-number"
                                    v-model.number="data.documentNumber" step="1"
                                    class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6" />
                            </div>
                            <!-- Carga de imagen -->
                            <div class="col-span-6 sm:col-span-3">
                                <label class="block text-sm font-medium leading-6 text-gray-900">Foto documento —
                                    Frente</label>
                                <div
                                    class="mt-2 flex justify-center rounded-md border-2 border-dashed border-gray-300 px-6 pt-2 pb-3">
                                    <div class="space-y-1 text-center">
                                        <svg class="mx-auto h-12 w-12 text-gray-400" stroke="currentColor" fill="none"
                                            viewBox="0 0 48 48" aria-hidden="true">
                                            <path
                                                d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02"
                                                stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                                        </svg>
                                        <div class="flex text-sm text-gray-600">
                                            <label for="file-upload"
                                                class="relative cursor-pointer rounded-md bg-white font-medium text-indigo-600 focus-within:outline-none focus-within:ring-2 focus-within:ring-indigo-500 focus-within:ring-offset-2 hover:text-indigo-500">
                                                <span>Cargar un archivo</span>
                                                <input id="file-upload" required="required" name="file-upload" type="file"
                                                    class="sr-only" v-on:change="uploadDocumen($event)">
                                            </label>
                                        </div>
                                        <p class="text-xs text-gray-500">JPG hasta 10MB</p>
                                    </div>
                                </div>
                            </div>

                            <div class="col-span-6 sm:col-span-3">
                                <label class="block text-sm font-medium leading-6 text-gray-900">Foto documento —
                                    Reverso</label>
                                <div
                                    class="mt-2 flex justify-center rounded-md border-2 border-dashed border-gray-300 px-6 pt-2 pb-3">
                                    <div class="space-y-1 text-center">
                                        <svg class="mx-auto h-12 w-12 text-gray-400" stroke="currentColor" fill="none"
                                            viewBox="0 0 48 48" aria-hidden="true">
                                            <path
                                                d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02"
                                                stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                                        </svg>
                                        <div class="flex text-sm text-gray-600">
                                            <label for="file-upload-Back"
                                                class="relative cursor-pointer rounded-md  bg-white font-medium text-indigo-600 focus-within:outline-none focus-within:ring-2 focus-within:ring-indigo-500 focus-within:ring-offset-2 hover:text-indigo-500">
                                                <span>Cargar un archivo</span>
                                                <input id="file-upload-Back" required="required" name="file-upload-Back"
                                                    type="file" class="sr-only" v-on:change="uploadDocumenBack($event)">
                                            </label>

                                        </div>
                                        <p class="text-xs text-gray-500">JPG hasta 10MB</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="bg-gray-50 px-4 py-3 text-right sm:px-6">
                        <button @click.prevent="gotoStep(2)"
                            class="inline-flex justify-center w-22 rounded-md bg-indigo-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">
                            Continuar
                        </button>
                    </div>
                </div>
            </div>
            <ModalForm v-if="showModalValidateOne" :title="dataTitle" :section="dataSection"></ModalForm>
            <!-- Formulario 2 -->
            <div class="overflow-hidden drop-shadow-lg sm:rounded-xl" v-if="activeForm == 2">
                <div class="bg-white px-4 py-5 sm:p-6">
                    <div class="grid grid-cols-6 gap-6">
                        <div class="col-span-6 sm:col-span-4">
                            <label for="email-address" class="block text-sm font-medium leading-6 text-gray-900">Correo
                                electrónico
                            </label>
                            <input type="email" name="email-address" id="email-address" autocomplete="email"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.emailAddress">
                        </div>
                        <div class="col-span-6 sm:col-span-3">
                            <label for="password" class="block text-sm font-medium leading-6 text-gray-900">
                                Contraseña
                            </label>
                            <input type="password" required="required" name="password" id="password"
                                autocomplete="current-password"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.password">
                        </div>
                        <div class="col-span-6 sm:col-span-3">
                            <label for="repeat-password" class="block text-sm font-medium leading-6 text-gray-900">
                                Confirmación de Contraseña
                            </label>
                            <input type="password" required="required" name="repeat-password" id="repeat-password"
                                autocomplete="current-password"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.repeatPassword">
                        </div>
                        <div class="col-span-6 sm:col-span-3">
                            <label for="phone" class="block text-sm font-medium leading-6 text-gray-900">
                                Numero teléfono</label>
                            <input type="number" name="phone" id="phone" autocomplete="tel"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.phone">
                        </div>
                        <div class="col-span-6 sm:col-span-3">
                            <label for="phone-movil" class="block text-sm font-medium leading-6 text-gray-900">
                                Numero celular</label>
                            <input type="number" name="phone-movil" id="phone-movil" autocomplete="tel"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 px-4 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.phoneMovil">
                        </div>
                    </div>
                </div>
                <div class="flex justify-between bg-gray-50 px-4 py-3 text-right sm:px-6">
                    <button @click.prevent="gotoStep(1)"
                        class="inline-flex justify-center w-22 rounded-md bg-indigo-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">
                        Atras
                    </button>
                    <button @click.prevent="gotoStep(3)"
                        class="inline-flex justify-center w-22 rounded-md bg-indigo-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">
                        Continuar
                    </button>
                </div>
            </div>
            <ModalForm v-if="showModalValidateTwo" :title="dataTitle" :section="dataSection"></ModalForm>
            <!-- Formulario 3 -->
            <div class="overflow-hidden drop-shadow-lg sm:rounded-xl" v-if="activeForm == 3">
                <div class="bg-white px-4 py-5 sm:p-6">
                    <div class="grid grid-cols-6 gap-6">
                        <div class="col-span-4">
                            <label for="street-address" class="block text-sm font-medium leading-6 text-gray-900">Dirección
                                residencia</label>
                            <input type="text" name="street-address" id="street-address" autocomplete="street-address"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.streetAddress">
                        </div>
                        <div class="col-span-6 sm:col-span-3 lg:col-span-2">
                            <label for="postal-code" class="block text-sm font-medium leading-6 text-gray-900">Código
                                postal</label>
                            <input type="text" name="postal-code" id="postal-code" autocomplete="postal-code"
                                class="mt-2 block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
                                v-model="data.postalCode">
                        </div>
                    </div>
                </div>
                <div class="flex justify-between bg-gray-50 px-4 py-3 text-right sm:px-6">
                    <button @click.prevent="gotoStep(2)"
                        class="inline-flex justify-center w-22 rounded-md bg-indigo-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">
                        Atras
                    </button>
                    <button type="submit"
                        class="inline-flex justify-center w-22 rounded-md bg-indigo-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">
                        Enviar
                    </button>
                </div>

            </div>
            <ModalForm v-if="showModalValidateThree" :title="dataTitle" :section="dataSection"></ModalForm>
            <ModalForm v-if="showModal" :title="dataTitle" :section="dataSection" ></ModalForm>
        </form>
    </div>
</template>

<style scoped></style>
