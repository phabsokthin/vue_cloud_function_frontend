<template>
  <div class="w-[40%] mx-auto mt-10 bg-white shadow-md">
    <div class="p-4">
      <p class="font-bold text-lg my-4">ចុះឈ្មោះសិស្ស</p>
      <form @submit.prevent="submitForm" class="space-y-2">
        <!-- First Name -->
        <div class="flex flex-col">
          <label for="" class="font-bold text-lg">First Name</label>
          <input type="text" v-model="firstName" class="border-2 p-2 w-full outline-none focus:border-blue-500"
            placeholder="First Name">
        </div>

        <!-- Last Name -->
        <div class="flex flex-col">
          <label for="" class="font-bold text-lg">Last Name</label>
          <input type="text" v-model="lastName" class="border-2 p-2 w-full outline-none focus:border-blue-500"
            placeholder="Last Name">
        </div>

        <!-- Email -->
        <div class="flex flex-col">
          <label for="" class="font-bold text-lg">Email</label>
          <input type="email" v-model="email" class="border-2 p-2 w-full outline-none focus:border-blue-500"
            placeholder="Email">
        </div>

        <!-- Date Of Birth -->
        <div class="flex flex-col">
          <label for="" class="font-bold text-lg">Date Of Birth</label>
          <input type="date" v-model="dob" class="border-2 p-2 w-full outline-none focus:border-blue-500"
            placeholder="Date of birth">
        </div>

        <!-- Enroll Date -->
        <div class="flex flex-col">
          <label for="" class="font-bold text-lg">Enroll Date</label>
          <input type="date" required v-model="enroleDate"
            class="border-2 p-2 w-full outline-none focus:border-blue-500" placeholder="Enroll date">
        </div>

        <!-- Major -->
        <div class="flex flex-col">
          <label for="" class="font-bold text-lg">Major</label>
          <select required v-model="major" class="border-2 p-2 w-full outline-none focus:border-blue-500">
            <option value="IT">IT</option>
            <option value="Web Developer">Web Developer</option>
            <option value="Mobile Developer">Mobile Developer</option>
          </select>
        </div>

        <div class="flex justify-end gap-2">
          <RouterLink :to="{name: 'student_list'}" class="p-3 px-5 bg-green-600 text-white rounded-md hover:bg-green-500">List Student</RouterLink>
          <button class="p-3 px-5 bg-blue-600 text-white rounded-md hover:bg-blue-500">Submit</button>
        </div>
      </form>
      <p v-if="responseMessage" class="mt-4 text-green-600">{{ responseMessage }}</p>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const firstName = ref("");
    const lastName = ref("");
    const email = ref("");
    const dob = ref("");
    const enroleDate = ref("");
    const major = ref("");
    const responseMessage = ref("");

    const submitForm = async () => {
      const studentData = {
        firstName: firstName.value,
        lastName: lastName.value,
        email: email.value,
        dateOfBirth: dob.value,
        enrollmentDate: enroleDate.value,
        major: major.value
      };

      try {
        const response = await fetch('https://createstudentv2-olboqunkva-uc.a.run.app/createStudentV2', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(studentData)
        });

        if (response.ok) {
          responseMessage.value = 'Student created successfully!';
        } else {
          const errorText = await response.text();
          responseMessage.value = `Error: ${errorText}`;
        }
      } catch (error) {
        responseMessage.value = `Error: ${error.message}`;
      }
    };


    return { firstName, lastName, email, dob, enroleDate, major, submitForm, responseMessage };
  }
};
</script>
