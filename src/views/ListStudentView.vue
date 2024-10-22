<template>
    <div class="w-[80%] mx-auto mt-10 bg-white shadow-md p-4">
        <p class="font-bold text-lg ">Student List</p>
        <div class="my-4">
            <RouterLink to="/" class="p-2 px-5 bg-red-600 text-white rounded-md hover:bg-red-500">Back</RouterLink>
        </div>
        <table class="table-auto w-full border-collapse border border-gray-400">
            <thead>
                <tr class="bg-gray-200">
                    <th class="border border-gray-300 p-2">First Name</th>
                    <th class="border border-gray-300 p-2">Last Name</th>
                    <th class="border border-gray-300 p-2">Email</th>
                    <th class="border border-gray-300 p-2">Date of Birth</th>
                    <th class="border border-gray-300 p-2">Enrollment Date</th>
                    <th class="border border-gray-300 p-2">Major</th>
                    <th class="border border-gray-300 p-2">Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="student in students" :key="student.email" class="hover:bg-gray-100">
                    <td class="border border-gray-300 p-2">{{ student.claims.firstName }}</td>
                    <td class="border border-gray-300 p-2">{{ student.claims.lastName }}</td>
                    <td class="border border-gray-300 p-2">{{ student.email }}</td>
                    <td class="border border-gray-300 p-2">{{ student.claims.dateOfBirth }}</td>
                    <td class="border border-gray-300 p-2">{{ student.claims.enrollmentDate }}</td>
                    <td class="border border-gray-300 p-2">{{ student.claims.major }}</td>
                    <td>
                        <div class="space-x-2">
                            <button @click="handleDelete(student.uid)"
                                class="p-2 bg-red-500 text-sm text-white rounded-lg hover:bg-red-600 transition-all duration-300 ease-in-out ">Delete</button>
                            <RouterLink :to="{ name: 'viewStudentDetail', params: { id: student.uid } }"
                                class="p-2 bg-blue-500 text-sm text-white rounded-lg hover:bg-blue-600 transition-all duration-300 ease-in-out ">
                                View Data
                            </RouterLink>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>

</template>

<script>
import { ref, onMounted } from 'vue';

export default {
    setup() {
        const students = ref([]);

        const fetchStudent = async () => {
            try {
                const response = await fetch('https://getallstudentclaimsv2-olboqunkva-uc.a.run.app/getAllStudentClaimsV2', {
                    method: 'GET',

                    headers: {
                        'Content-Type': 'application/json',
                    },
                });

                if (!response.ok) {
                    throw new Error(`Error: ${response.statusText}`);
                }
                const data = await response.json();

                if (data && data.students) {
                    students.value = data.students;
                }
            } catch (error) {
                console.error('Error fetching students:', error);
            }
        };
        const handleDelete = async (uid) => {
            if (window.confirm("Are you sure you want to delete this student?")) {
                try {
                    const response = await fetch(`https://deletestudentv2-olboqunkva-uc.a.run.app/deleteStudentV2`, {
                        method: 'DELETE',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({ uid }),
                    });

                    if (!response.ok) {
                        throw new Error(`Error: ${response.statusText}`);
                    }

                    const message = await response.text();
                    alert(message);
                    fetchStudent();
                } catch (error) {
                    console.error('Error deleting student:', error);
                    alert("Delete failed, please try again.");
                }
            }
        };

        onMounted(() => {
            fetchStudent();
        });

        return { students, handleDelete };
    }
};
</script>
