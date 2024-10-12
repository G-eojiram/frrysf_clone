<template>
    <TransitionRoot as="template" :show="open">
        <Dialog as="div" class="relative z-30" @close="open = false">
            <TransitionChild as="template" enter="ease-out duration-300" enter-from="opacity-0" enter-to="opacity-100"
                leave="ease-in duration-200" leave-from="opacity-100" leave-to="opacity-0">
                <div class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity" />
            </TransitionChild>
            <div class="fixed inset-0 z-10 overflow-y-auto">
                <div class="flex min-h-full items-end justify-center p-4 text-center sm:items-center sm:p-0">
                    <TransitionChild as="template" enter="ease-out duration-300"
                        enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
                        enter-to="opacity-100 translate-y-0 sm:scale-100" leave="ease-in duration-200"
                        leave-from="opacity-100 translate-y-0 sm:scale-100"
                        leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95">
                        <DialogPanel
                            class="relative transform overflow-hidden rounded-lg bg-white px-4 pb-4 pt-5 text-left shadow-xl transition-all sm:my-8 sm:w-fit sm:max-w-[40rem] sm:p-6">
                            <div class="sm:flex sm:items-start">
                                <div class="mt-3 text-center sm:mt-0 sm:text-left w-full">
                                    <div class="flex justify-end">
                                        <button @click="open = false" ref="cancelButtonRef">
                                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none"
                                                stroke="currentColor" stroke-width="2" class="w-4 h-4">
                                                <path stroke-linecap="round" stroke-linejoin="round"
                                                    d="M6 18L18 6M6 6l12 12" />
                                            </svg>
                                        </button>
                                    </div>
                                    <DialogTitle as="h3"
                                        class="mb-[1.5rem] text-[1.3rem] font-semibold leading-6 text-gray-700 flex justify-center">
                                        Create Report
                                    </DialogTitle>
                                    <div class="overflow-hidden text-gray-700 flex flex-col gap-y-4 mx-4">
                                        <div class="flex items-center gap-x-3">
                                            <img class="h-16 w-16 border border-gray-500 rounded-full object-cover"
                                                :src="profileUrl" alt="profile image " />
                                            <div class="flex flex-col gap-y-1">
                                                <span class="text-base font-medium ">Reporter Name</span>
                                                <select
                                                    class="block w-fit text-sm border rounded-lg p-1 px-4 text-gray-700 bg-slate-100">
                                                    <option value="" selected disabled hidden>Report Type</option>
                                                    <option value="Missing">Missing Pet</option>
                                                    <option value="Abandoned">Abandoned Pet</option>
                                                </select>
                                            </div>
                                        </div>
                                        <div class="text-sm">
                                            <div class="py-2 flex flex-col gap-y-2">
                                                <label for="petcategory" class="font-medium">Pet Category</label>
                                                <select id="petcategory"
                                                    class="block border rounded-lg w-full text-sm py-1.5 px-4 text-gray-700 bg-slate-50">
                                                    <option value="" selected disabled hidden>Select Pet Category
                                                    </option>
                                                    <option value="Dog">Dog</option>
                                                    <option value="Cat">Cat</option>
                                                    <option value="Others">Others</option>
                                                </select>
                                            </div>
                                            <div class="py-2 flex flex-col gap-y-2">
                                                <label for="location" class="font-medium">Location</label>
                                                <input id="location" placeholder="Enter your Location"
                                                    class="border rounded-lg py-2 px-4" />
                                            </div>
                                            <div class="py-2 flex flex-col gap-y-2">
                                                <label for="petcondition" class="font-medium">Pet Condition</label>
                                                <textarea name="petcondition" id="petcondition"
                                                    placeholder="Animal Status" class="border rounded-lg py-2 px-4" />
                                            </div>
                                            <label for="Report Details" class="font-medium">
                                                Report Details</label>
                                            <div class="flex flex-col gap-y-2 border rounded-lg mt-2">
                                                <textarea name="reportdetails" id="reportdetails"
                                                    placeholder="Write a caption or a description about the pet situation"
                                                    class="py-2 px-4 rounded-lg" />
                                                <!--  to display images -->
                                                <div v-if="imageUrls.length > 0"
                                                    class="grid grid-cols-1 place-items-center gap-1 border-t">
                                                    <div v-for="(imageUrl, index) in imageUrls" :key="index"
                                                        class="relative mx-1">
                                                        <img :src="imageUrl" alt="Uploaded Image"
                                                            class=" max-w-full max-h-[300px] object-contain border" />
                                                        <button @click="removeImage(index)"
                                                            class="absolute top-0 right-0 p-1 text-red-500 hover:text-red-800">
                                                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"
                                                                fill="none" stroke="currentColor" stroke-width="2"
                                                                class="w-4 h-4">
                                                                <path stroke-linecap="round" stroke-linejoin="round"
                                                                    d="M6 18L18 6M6 6l12 12" />
                                                            </svg>
                                                        </button>
                                                    </div>
                                                </div>
                                            </div>
                                            <div class="flex justify-between my-3 mx-[1.5rem]">
                                                <div>
                                                    <span class="text-gray-400 text-[12px]">Add to your post</span>
                                                </div>
                                                <div class="flex gap-4 items-center">
                                                    <div>
                                                        <label for="file-input"
                                                            class="cursor-pointer flex gap-3 items-center">
                                                            <input type="file" multiple @change="handleFileChange"
                                                                id="file-input" ref="fileInput" class="hidden" />
                                                            <img :src="images" alt="Images Icon"
                                                                class="h-[2rem] w-[2rem]" />
                                                        </label>
                                                    </div>
                                                    <!-- <div> // pin location icon...
                                                        <button>
                                                            <svg xmlns="http://www.w3.org/2000/svg"
                                                                viewBox="0 0 384 512" width="25" height="25">
                                                                <path fill="#f03d3d"
                                                                    d="M172.3 501.7C27 291 0 269.4 0 192 0 86 86 0 192 0s192 86 192 192c0 77.4-27 99-172.3 309.7-9.5 13.8-29.9 13.8-39.5 0zM192 272c44.2 0 80-35.8 80-80s-35.8-80-80-80-80 35.8-80 80 35.8 80 80 80z" />
                                                            </svg>
                                                        </button>
                                                    </div> -->
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="flex justify-center w-full">
                                <button type="button"
                                    class="inline-flex w-full justify-center rounded-md bg-red-500 px-[12.2rem] py-2 text-sm font-semibold text-white shadow-sm hover:bg-bgteal sm:ml-3 sm:w-auto"
                                    @click="open = false">Report</button>
                            </div>

                        </DialogPanel>
                    </TransitionChild>
                </div>
            </div>
        </Dialog>
    </TransitionRoot>
</template>
<script>
import { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot } from '@headlessui/vue'

export default {
    components: { Dialog, DialogPanel, DialogTitle, TransitionChild, TransitionRoot },
    data() {
        return {
            open: true,
            fileInput: null,
            imageUrls: [],
            images: 'https://img.icons8.com/fluency/48/stack-of-photos.png',
        }
    },
    methods: {
        handleFileChange(event) {
            const files = event.target.files;
            for (let i = 0; i < files.length; i++) {
                const file = files[i];
                const reader = new FileReader();
                reader.onload = (event) => {
                    this.imageUrls.push(event.target.result);
                };
                reader.readAsDataURL(file);
            }
        },
        removeImage(index) {
            this.imageUrls.splice(index, 1);
        },
    }
}
</script>