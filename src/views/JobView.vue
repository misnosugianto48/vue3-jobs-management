<template>
  <BackButton />

  <section v-if="!state.isLoading" class="bg-green-50">
    <div class="container m-auto py-10 px-6">
      <div class="grid grid-cols-1 md:grid-cols-70/30 w-full gap-6">
        <main>
          <div class="bg-white p-6 rounded-lg shadow-md text-center md:text-left">
            <div class="text-gray-500 mb-4">{{ state.job.type }}</div>
            <h1 class="text-3xl font-bold mb-4">{{ state.job.title }}</h1>
            <div class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start">
              <i class="pi pi-map-marker text-lg text-orange-700 mr-2"></i>
              <p class="text-orange-700">{{ state.job.location }}</p>
            </div>
          </div>

          <div class="bg-white p-6 rounded-lg shadow-md mt-6">
            <h3 class="text-green-800 text-lg font-bold mb-6">
              Job Description
            </h3>

            <p class="mb-4">
              {{ state.job.description }}
            </p>

            <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>

            <p class="mb-4">{{ state.job.salary }} / Year</p>
          </div>
        </main>

        <!-- Sidebar -->
        <aside>
          <!-- Company Info -->
          <div class="bg-white p-6 rounded-lg shadow-md">
            <h3 class="text-xl font-bold mb-6">Company Info</h3>

            <h2 class="text-2xl">{{ state.job.company.name }}</h2>

            <p class="my-2">
              {{ state.job.company.description }}
            </p>

            <hr class="my-4" />

            <h3 class="text-xl">Contact Email:</h3>

            <p class="my-2 bg-green-100 p-2 font-bold">
              {{ state.job.company.contactEmail }}
            </p>

            <h3 class="text-xl">Contact Phone:</h3>

            <p class="my-2 bg-green-100 p-2 font-bold">{{ state.job.company.contactPhone }}</p>
          </div>

          <!-- Manage -->
          <div class="bg-white p-6 rounded-lg shadow-md mt-6">
            <h3 class="text-xl font-bold mb-6">Manage Job</h3>
            <RouterLink :to="`/jobs/edit/${state.job.id}`"
              class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
              Edit
              Job</RouterLink>
            <button type="button" id="deleteButton" @click="openModal"
              class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
              Delete Job
            </button>
          </div>
        </aside>
      </div>
    </div>
  </section>

  <!-- Show Spinner Loading is true -->
  <div v-else class="text-center text-gray-500 py-6">
    <PulseLoader />
  </div>

  <!-- modal delete -->
  <div id="deleteModal" v-if="showDeleteModal" tabindex="-1" aria-hidden="true"
    class="fixed inset-0 z-50 flex items-center justify-center">
    <div class="relative p-4 w-full max-w-md h-full md:h-auto">
      <!-- Modal content -->
      <div class="relative p-4 text-center bg-green-200 rounded-lg shadow dark:bg-gray-800 sm:p-5">
        <button type="button" @click="closeModal"
          class="text-gray-400 absolute top-2.5 right-2.5 bg-transparent hover:bg-dark-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white">
          <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd"
              d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 011.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
              clip-rule="evenodd"></path>
          </svg>
          <span class="sr-only">Close modal</span>
        </button>
        <svg class="text-gray-400 dark:text-dark-500 w-11 h-11 mb-3.5 mx-auto" aria-hidden="true" fill="currentColor"
          viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd"
            d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
            clip-rule="evenodd"></path>
        </svg>
        <p class="mb-4 text-dark-500 dark:text-dark-300">Are you sure you want to delete this item?</p>
        <div class="flex justify-center items-center space-x-4">
          <button @click="closeModal" type="button"
            class="py-2 px-3 text-sm font-medium text-gray-500 bg-white rounded-lg border border-gray-200 hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-primary-300 hover:text-gray-900 focus:z-10 dark:bg-gray-700 dark:text-gray-300 dark:border-gray-500 dark:hover:text-white dark:hover:bg-gray-600 dark:focus:ring-gray-600">
            No, cancel
          </button>
          <button @click="deleteJob"
            class="py-2 px-3 text-sm font-medium text-center text-white bg-red-600 rounded-lg hover:bg-red-700 focus:ring-4 focus:outline-none focus:ring-red-300 dark:bg-red-500 dark:hover:bg-red-600 dark:focus:ring-red-900">
            Yes, I'm sure
          </button>
        </div>
      </div>
    </div>
  </div>

</template>

<script setup>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import axios from 'axios';
import { reactive, onMounted, ref } from 'vue';
import { useRoute, RouterLink, useRouter } from 'vue-router';
import BackButton from '@/components/BackButton.vue';
import { useToast } from 'vue-toastification';

const route = useRoute();
const router = useRouter();
const toast = useToast();
const jobId = route.params.id;

const state = reactive({
  job: {},
  isLoading: true,
});

const showDeleteModal = ref(false);

const openModal = () => {
  showDeleteModal.value = true;
};

const closeModal = () => {
  showDeleteModal.value = false;
};


const deleteJob = async () => {
  try {
    await axios.delete(`/api/jobs/${jobId}`);
    toast.success('Job deleted successfully');
    router.push('/jobs');
  } catch (error) {
    console.error('Error delete job', error);
    toast.error('Failed delete job');
  }
}

onMounted(async () => {
  try {
    const response = await axios.get(`/api/jobs/${jobId}`);
    state.job = response.data;
  } catch (error) {
    console.error('Error fetching jobs', error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<style lang="scss" scoped></style>