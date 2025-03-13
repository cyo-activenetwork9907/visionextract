<script setup>
import { useToast } from 'primevue/usetoast';
import { onMounted, ref } from 'vue';

const toast = useToast();
const fileupload = ref();
const files_list = ref([]);

onMounted(() => {
    // 读取本地./file里的文件
    files_list.value = [];
});

function onUpload(event) {
    toast.add({ severity: 'info', summary: 'Success', detail: 'ファイルアップロード成功', life: 3000 });
    console.log(event.files);
    files_list.value =  event.files;
    // 将文件储存到本地
}
</script>

<template>
    <div class="grid grid-cols-12 gap-8">
        <div class="col-span-full lg:col-span-6">
            <div class="card">
                <div class="font-semibold text-xl mb-4">ファイルアプロード</div>
                <FileUpload name="demo[]" @uploader="onUpload" :multiple="true" accept="image/*" :maxFileSize="10000000" customUpload />
            </div>
        </div>
        <div class="col-span-full lg:col-span-6">
            <div class="card">
                <div class="font-semibold text-xl mb-4">ファイルリスト</div>
                <DataView  v-if="files_list.length!=0" :value="files_list" >
                    <template #list="slotProps">
                        <div class="flex flex-col">
                            <div v-for="(item, index) in slotProps.items" :key="index">
                                <div class="flex flex-col sm:flex-row sm:items-center p-6 gap-4" :class="{ 'border-t border-surface': index !== 0 }">
                                    <div class="md:w-40 relative">
                                        <img class="block xl:block mx-auto rounded w-full" :src="`${item.objectURL}`" :alt="item.name" />
                                    </div>
                                    <div class="flex flex-col md:flex-row justify-between md:items-center flex-1 gap-6">
                                        <div class="flex flex-row md:flex-col justify-between items-start gap-2">
                                            <div>
                                                <div class="text-lg font-medium mt-2">{{ item.name }}</div>
                                            </div>
                                            <div class="bg-surface-100 p-1" style="border-radius: 30px">
                                            
                                            </div>
                                        </div>
                                        <div class="flex flex-col md:items-end gap-8">
                                            <div class="flex flex-row-reverse md:flex-row gap-2">
                                                <i class="pi pi-spin pi-spinner" style="font-size: 2rem"></i>
                                                <Button label="詳細" >
                                                    <RouterLink :to="`/pdf_list/${item.name}/pdf_detail`" :class="slotProps.class">Router</RouterLink>
                                                </Button>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </template>
                </DataView>
            </div>
        </div>
    </div>
</template>


