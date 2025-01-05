<template>
  <div class="bg-slate-900 text-white min-h-screen">
    <header class="bg-sky-400 p-4">
      <h1 class="text-center text-2xl font-bold">Carcols & Modkit ID Resolver</h1>
    </header>

    <main class="container mx-auto p-4 space-y-6">
      <!-- Navigation for switching between sections -->
      <nav class="flex justify-center space-x-4 mb-6">
        <UButton @click="currentSection = 'carcols'" :variant="currentSection === 'carcols' ? 'solid' : 'outline'" >Carcols Resolver</UButton>
        <UButton @click="currentSection = 'modkit'" :variant="currentSection === 'modkit' ? 'solid' : 'outline'" >Modkit Resolver</UButton>
      </nav>

      <!-- Conditional Rendering for Sections -->
      <div v-if="currentSection === 'carcols'">
        <UCard>
          <template #header>
            <h2 class="text-xl font-semibold">Carcols Confliction Resolution</h2>
          </template>

          <UForm class="space-y-4">
            <UInput v-model="carcolsFileInput" type="file" size="sm" icon="i-heroicons-folder" accept=".meta" required label="Upload carcols.meta" />
            <UInput v-model="carvariationsFileInput" type="file" size="sm" icon="i-heroicons-folder" accept=".meta" required label="Upload carvariations.meta" />

            <UInputMenu v-model="selectedVehicle" :options="vehicles" label="Select Vehicle Scope" />

            <UButton @click="resolveCarcols" size="lg" >Resolve Carcol IDs</UButton>
          </UForm>
        </UCard>
      </div>

      <div v-else-if="currentSection === 'modkit'">
        <UCard>
          <template #header>
            <h2 class="text-xl font-semibold">Modkit ID Confliction Resolution</h2>
          </template>

          <UForm class="space-y-4">
            <UInput v-model="modkitCarcolsFileInput" type="file" size="sm" icon="i-heroicons-folder" accept=".meta" required label="Upload carcols.meta" />
            <UInput v-model="modkitCarvariationsFileInput" type="file" size="sm" icon="i-heroicons-folder" accept=".meta" required label="Upload carvariations.meta" />

            <UInputMenu v-model="selectedModkitVehicle" :options="modkitVehicles" label="Select Vehicle Scope" />

            <UButton @click="resolveModkit" size="lg" >Resolve Modkit IDs</UButton>
          </UForm>
        </UCard>
      </div>
    </main>

    <!-- Modal for Notifications -->
    <UModal v-model="isModalOpen">
      <UCard>
        <template #header>
          <h2 class="text-xl font-semibold">Notification</h2>
        </template>
        <p class="p-4 text-white">{{ modalMessage }}</p>
        <template #footer>
          <UButton  @click="isModalOpen = false">Close</UButton>
        </template>
      </UCard>
    </UModal>

    <footer class="bg-slate-900 p-4 text-center">
      <p>&copy; 2025 Carcols & Modkit Resolver. All Rights Reserved.</p>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// State for navigation
const currentSection = ref('carcols');

// Modal State
const isModalOpen = ref(false);
const modalMessage = ref('');

// Shared states
const vehicles = ['All vehicles', 'Single vehicle'];
const selectedVehicle = ref(vehicles[0]);
const modkitVehicles = ['All vehicles', 'Single vehicle'];
const selectedModkitVehicle = ref(modkitVehicles[0]);

// File input states
const carcolsFileInput = ref(null);
const carvariationsFileInput = ref(null);
const modkitCarcolsFileInput = ref(null);
const modkitCarvariationsFileInput = ref(null);

// Helper functions
function resolveCarcols() {
  if (!carcolsFileInput.value || !carvariationsFileInput.value) {
    modalMessage.value = 'Please upload both carcols.meta and carvariations.meta files.';
    isModalOpen.value = true;
    return;
  }

  modalMessage.value = `Carcols IDs resolved for ${selectedVehicle.value}!`;
  isModalOpen.value = true;
}

function resolveModkit() {
  if (!modkitCarcolsFileInput.value || !modkitCarvariationsFileInput.value) {
    modalMessage.value = 'Please upload both carcols.meta and carvariations.meta files.';
    isModalOpen.value = true;
    return;
  }

  modalMessage.value = `Modkit IDs resolved for ${selectedModkitVehicle.value}!`;
  isModalOpen.value = true;
}
</script>

<style scoped>
.bg-gray-900 {
  background-color: #1a202c;
}
.text-white {
  color: #ffffff;
}
.bg-sky-400 {
  background-color: #38bdf8;
}
.bg-gray-700 {
  background-color: #2d3748;
}
</style>
