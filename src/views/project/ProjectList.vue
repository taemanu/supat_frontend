<style>
	.btn-equal-size {
		width: 90px;
	}
</style>
<template>

  <main class="content">
      <div class="container-fluid p-0 ">
        <div class="d-flex justify-content-between m-2" >
          <h1 class="h3 mb-4"><strong>โครงการ</strong></h1>
          <!-- <router-link class="btn btn-success mt-2" to="/app/FormProject">
            <i class="bi bi-plus"></i>สร้างโครงการ
          </router-link> -->
        </div>
      </div>
      <div class="card">
        <div class="card-body">
          <h1 class="h3 mb-4"><strong>โครงการ</strong></h1>
    
          <h2 class="h4 mb-3">โครงการที่กำลังดำเนินการ</h2>
          <div class="table-responsive mb-4">
            <table class="table table-bordered mb-0">
              <thead>
                <tr>
                  <th class="text-center">รหัสโครงการ</th>
                  <th class="text-center">เลขใบเสนอราคา</th>
                  <th class="text-center">ชื่อโครงการ</th>
                  <th class="text-center">วันที่เริ่มสัญญา</th>
                  <th class="text-center">กำหนดส่งงาน</th>
                  <th class="text-center">แบบงาน</th>
                  <th class="text-center">ดำเนินการไปแล้ว</th>
                  <th class="text-center">จัดการ</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(list, index) in project_store.data_list_pedding" :key="index" >
                  <td class="text-center">{{ list.p_code }}</td>
                  <td class="text-center">{{ list.id_qt }}</td>
                  <td>{{ list.p_name }}</td>
                  <td class="text-center">{{ list.date_start }}</td>
                  <td class="text-center">{{ list.date_end }}</td>
                  <td class="text-center">xxx.pdf</td>
                  <td class="text-center">{{ list.percent }}%</td>
                  <td class="text-center">
                    <button type="button" class="btn btn-primary me-2" @click="showTaskDetails(list.id,list.p_name,list.percent)">
                      <i class="fas fa-plus"></i> จัดการงาน
                    </button>
    
                    <router-link  class="btn btn-warning me-2" :to="{ path: '/app/ProjectPeriod', query: { id: list.id , id_qt:  list.id_qt ,  p_code:  list.p_code} }">
                      <i class="fas fa-plus"></i> จัดการค่างวด
                    </router-link>
                    

                    <router-link  class="btn btn btn-info" :to="{ path: '/app/ProjectMaterialList', query: { id: list.id , p_name:  list.p_name ,  p_code:  list.p_code} }" >
                      <i class="fas fa-plus"></i> สั่งซื้อวัสดุ
                    </router-link>
                    <!-- <button type="button" class="btn btn-info me-2">
                    </button> -->
                    <!-- <button type="button" class="btn btn-primary me-2" @click="change_status(list.id,'approve')">
                      <i class="fas fa-plus"></i> อนุมัติ
                    </button>
    
                    <button type="button" class="btn btn-danger" @click="change_status(list.id,'cancel')">
                      <i class="fas fa-plus"></i> ไม่อนุมัติ
                    </button> -->
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
    
          <h2 class="h4 mb-3">โครงการที่เสร็จสิ้น</h2>
          <div class="table-responsive">
            <table class="table table-bordered mb-0">
              <thead>
                <tr>
                  <th class="text-center">รหัสโครงการ</th>
                  <th class="text-center">เลขใบเสนอราคา</th>
                  <th class="text-center">ชื่อโครงการ</th>
                  <th class="text-center">วันที่เริ่มสัญญา</th>
                  <th class="text-center">กำหนดส่งงาน</th>
                  <th class="text-center">สัญญา</th>
                  <th class="text-center">แบบงาน</th>
                  <th class="text-center">สถานะ</th>
                  <th class="text-center">จัดการ</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(list2, index2) in project_store.data_list" :key="index2" >
                  <td class="text-center">{{ list2.p_code }}</td>
                  <td class="text-center">{{ list2.id_qt }}</td>
                  <td>
                    <a href="#" @click="showProjectDetails(list2.p_code)">{{ list2.p_name}}</a>
                  </td>
                  <td class="text-center">{{ list2.date_start }}</td>
                  <td class="text-center">{{ list2.date_end }}</td>
                  <td class="text-center">file1.pdf</td>
                  <td class="text-center">file2.pdf</td>
                  <td class="text-center">
                    <span class="text-success" v-if="list2.status == 'success'">
                      เสร็จสิ้น
                    </span>
    
                    <span class="text-danger" v-if="list2.status == 'reject'">
                      ไม่อนุมัติ
                    </span>
                  </td>
                  <td class="text-center">
                    <button type="button" class="btn btn-warning me-2" @click="showTaskDetails(list2.id,list2.p_name,list2.percent)">
                      <i class="fas fa-plus"></i> งาน
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>


      <!-- โคงการ -->
      <div class="modal fade" id="projectModal" tabindex="-1" aria-labelledby="projectModalLabel" aria-hidden="true" ref="projectModal">
        <div class="modal-dialog modal-lg modal-dialog-centered">  <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="projectModalLabel">{{ modalContent.name }}</h5>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
              <ul class="list-group">  <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>รหัสโครงการ:</strong>
                  <span class="badge bg-primary rounded-pill">{{ modalContent.code_p }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>ชื่อโครงการ:</strong>
                  <span>{{ modalContent.name }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>เลขใบเสนอราคา:</strong>
                  <span>{{ modalContent.id_qt }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>วันที่เริ่มสัญญา:</strong>
                  <span>{{ modalContent.date_start }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>กำหนดส่งงาน:</strong>
                  <span>{{ modalContent.date_end }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>สถานะ:</strong>
                  <span>{{ modalContent.status }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong>ความคืบหน้า:</strong>
                  <span>{{ modalContent.progress }}</span>
                </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong for="file1">สัญญา:</strong>  <a :href="modalContent.file1" download>{{ modalContent.file1 }}</a>  </li>
                <li class="list-group-item d-flex justify-content-between align-items-center">
                  <strong for="file2">แบบงาน:</strong>
                  <a :href="modalContent.file2" download>{{ modalContent.file2 }}</a>
                </li>
              </ul>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            </div>
          </div>
        </div> 
      </div>

      <!-- งาน-->
      <div class="modal fade" id="TaskModal" tabindex="-1" aria-labelledby="TaskModalLabel" aria-hidden="true" ref="TaskModal">
        <div class="modal-dialog modal-lg modal-dialog-centered">  <div class="modal-content">
            <div class="modal-header">
              <h3 class="modal-title" id="projectModalLabel">โครงการ {{ taskContent.name }}</h3>
              <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">

              <div>
                <strong style="margin-right: 10px; flex-basis: 20%; white-space: nowrap;">ดำเนินการไปแล้ว :</strong>

                <div class="progress" style="flex-grow: 1; margin-right: 10px; height: 30px;"> 
                  <div class="progress-bar" role="progressbar" :style="{ width: taskContent.percent + '%' }" style="background-color: green; height: 100%;" :aria-valuenow="taskContent.percent" aria-valuemin="0" aria-valuemax="100">
                    {{ taskContent.percent }}%
                  </div>
                </div>
              </div>

              <hr>
            
              <div v-for="(task, index) in task_data" :key="index" class="task-item">
                <div style="display: flex; align-items: center; margin-bottom: 10px;">
 
                  <strong style="margin-right: 10px; flex-basis: 20%; white-space: nowrap;">{{index+1}} . {{ task.task_name }} :</strong>
                  

                  <div class="progress" style="flex-grow: 1; margin-right: 10px; " v-if="task.status != 'success'">
                    <div class="progress-bar" role="progressbar"                    
                    style="width: 0%;"
                    :aria-valuenow="0"  
                    aria-valuemin="0" aria-valuemax="100">
                      0%
                    </div>
                  </div>

                  <div class="progress" style="flex-grow: 1; margin-right: 10px;" v-else>
                    <div class="progress-bar" role="progressbar"                    
                    style="width: 100%; background-color: green;"
                    :aria-valuenow="100"  
                    aria-valuemin="0" aria-valuemax="100">
                      100%
                    </div>
                  </div>
                  

                  <div>
                    <button @click="completeTask(task.id,task.percent)" v-if="task.status != 'success'" class="btn btn-warning me-2">เสร็จสิ้น</button>
                    <button class="btn btn-success me-2" v-else>เสร็จสิ้น</button>
                  </div>
                </div>
              </div>
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
            </div>
          </div>
        </div> 
      </div>

  </main>

</template>

<script setup>
import { ref, reactive, computed, watch, onMounted } from "vue";
import { userAuthStore } from "@/stores/models/userAuthStore";
import { ProjectStore } from "@/stores/models/ProjectStore";
import { useRouter } from "vue-router";
import axios from "axios";
import Swal from "sweetalert2";

const project_store = ProjectStore();
const router = useRouter();

const modalContent = reactive({
  name: "",
  code_p: "",
  id_qt: "",
  date_start: "",
  date_end: "",
  status: "",
  progress: "",
  file1: "",
  file2: "",
});

const taskContent = reactive({
  name: "",
  percent: "",
});

const task_data = ref(null);

const headers = {
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${localStorage.getItem("access_token")}`,
  },
};

onMounted(async () => {
  await project_store.getDataList();
});

const change_status = async (id_project,type) => {
  const result = await Swal.fire({
    // title: 'คุณแน่ใจหรือไม่?',
    text: "คุณแน่ใจหรือไม่?", // "Are you sure?"
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#3085d6",
    cancelButtonColor: "#d33",
    confirmButtonText: "ยืนยัน", // "Confirm"
    cancelButtonText: "ยกเลิก", // "Cancel"
  });

  if (result.isConfirmed) {
    try {
      const response = await axios.post(project_store.url.change_status, {
        id_project,
        type,
      });

      if (response.status === 200) {
        Swal.fire({
          icon: "success",
          title: "Success",
          text: response.data.message,
        });
      }
    } catch (error) {
      if (error.response && error.response.status === 500) {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: error.response.data.message,
        });
      } else {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: "Something went wrong!",
        });
      }
    } finally {
      project_store.getDataList();
    }
  }
};

const showProjectDetails = async (p_code) => {

  const data = await project_store.getDataDetail(p_code);

  console.log(data.data.p_name)

    // Fetch or set the details of the project using pCode
  modalContent.code_p = data.data.p_code;  // Assign the project code
  modalContent.name = data.data.p_name;  // Assign the project name (fetch or set dynamically)
  modalContent.id_qt = data.data.id_qt;  // Example data, set as needed
  modalContent.date_start = data.data.date_start;  // Example date, replace with actual data
  modalContent.date_end = data.data.date_end;  // Example date, replace with actual data
  modalContent.status = 'กำลังดำเนินการ';  // Example status, replace with actual data
  modalContent.progress = "50%";  // Example progress, replace with actual data
  modalContent.file1 = "File1.pdf";  // Example file, replace with actual data
  modalContent.file2 = "File2.pdf";  // Example file, replace with actual data
  
  // Use Bootstrap's JavaScript to show the modal
  const modal = new bootstrap.Modal(document.getElementById('projectModal'));
  modal.show();
}

const showTaskDetails = async (id,name,percent) => {
  const data = await project_store.getDataDetail(id);
  task_data.value = data.data;

  updateTaskPercentages();
  

  taskContent.name = name;
  taskContent.percent = percent;
  const modal = new bootstrap.Modal(document.getElementById('TaskModal'));
  modal.show();
}


const updateTaskPercentages = () => {
  const totalTasks = task_data.value.length;
  if (totalTasks === 0) return; 

  const percentPerTask = 100 / totalTasks; 

  task_data.value = task_data.value.map(task => ({
    ...task,
    percent: percentPerTask.toFixed(2)
  }));

  console.log(task_data.value);
};

const completeTask = async (id_project,percent) => {
    const result = await Swal.fire({
    // title: 'คุณแน่ใจหรือไม่?',
    text: "คุณแน่ใจหรือไม่?", // "Are you sure?"
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#3085d6",
    cancelButtonColor: "#d33",
    confirmButtonText: "ยืนยัน", // "Confirm"
    cancelButtonText: "ยกเลิก", // "Cancel"
  });

  if (result.isConfirmed) {
    try {
      const response = await axios.post(project_store.url.update_percents, {
        id_project,
        percent
      });

      if (response.status === 200) {
        Swal.fire({
          icon: "success",
          title: "Success",
          text: response.data.message,
        });



      }
    } catch (error) {
      if (error.response && error.response.status === 500) {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: error.response.data.message,
        });
      } else {
        Swal.fire({
          icon: "error",
          title: "Oops...",
          text: "Something went wrong!",
        });
      }
    } finally {
      await project_store.getDataList();

      // Get the existing modal instance
      const modalElement = document.getElementById('TaskModal');
      const modal = bootstrap.Modal.getInstance(modalElement);
      
      // Check if the modal is already initialized and then hide it
      if (modal) {
        modal.hide();
      }
    }
  }
}





</script>

<style scoped>
.card {
  margin-bottom: 1rem;
}
.table th,
.table td {
  vertical-align: middle;
}
.table td {
  white-space: nowrap;
}
</style>
