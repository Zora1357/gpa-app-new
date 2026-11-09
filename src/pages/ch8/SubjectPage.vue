<template>
  <q-page class="q-pa-md">
    <q-banner v-if="errorMessage" class="bg-red-1 text-negative q-mb-md">
      {{ errorMessage }}
    </q-banner>
    <q-table
      :columns="columns"
      :rows="subject"
      row-key="id"
      :loading="loading"
      :rows-per-page-options="[5, 10, 0]"
      flat
      bordered
    />
  </q-page>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import { api } from '@/boot/axios'
const subject = ref([])
const loading = ref(true)
const errorMessage = ref('')
const columns = [
  { name: 'id', label: 'ID', field: 'id', align: 'left' },
  { name: 'code', label: 'รหัสวิชา', field: 'code', align: 'center' },
  { name: 'name', label: 'ชื่อวิชา', field: 'name', align: 'left' },
  { name: 'credit', label: 'หน่วยกิต', field: 'credit', align: 'right' },
  { name: 'score', label: 'คะแนน', field: 'score', align: 'right' },
]

onMounted(async () => {
  try {
    const res = await api.get('/subject')
    subject.value = res.data
  } catch (error) {
    console.error('Error fetching subjects:', error)
    errorMessage.value = 'ไม่สามารถโหลดข้อมูลวิชาได้ กรุณาตรวจสอบการเชื่อมต่อ API'
  } finally {
    loading.value = false
  }
})
</script>
