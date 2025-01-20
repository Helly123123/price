<template>
  <div class="price-tag-manager">
    <h2>Управление ценниками</h2>
    <div class="price-tags">
      <div class="price-tag" v-for="(tag, index) in priceTags" :key="index">
        <div class="tag-info">
          <p class="business-name">ИП БИРЮКОВА</p>
          <h4 class="product-name">{{ tag.productName }}</h4>
          <div class="promotion-banner">АКЦИЯ</div>
          <div class="prices">
            <span class="old-price">{{ tag.oldPrice }} ₽</span>
            <span class="new-price">{{ tag.productPrice }} ₽</span>
          </div>
          <p class="date">{{ tag.date }}</p>
          <!-- Дата отображается здесь -->
        </div>
        <button @click="openEditModal(tag, index)">Редактировать</button>
      </div>
    </div>

    <!-- Модальное окно для редактирования ценника -->
    <div v-if="isEditModalOpen" class="edit-modal">
      <div class="edit-content">
        <h3>Редактировать ценник</h3>
        <div class="input-group">
          <label for="editProductName">Название продукта:</label>
          <input
            type="text"
            id="editProductName"
            v-model="currentTag.productName"
            placeholder="Введите название продукта"
          />
        </div>
        <div class="input-group">
          <label for="editOldPrice">Старая цена:</label>
          <input
            type="number"
            id="editOldPrice"
            v-model="currentTag.oldPrice"
            placeholder="Введите старую цену"
          />
        </div>
        <div class="input-group">
          <label for="editProductPrice">Новая цена:</label>
          <input
            type="number"
            id="editProductPrice"
            v-model="currentTag.productPrice"
            placeholder="Введите новую цену"
          />
        </div>
        <div class="input-group">
          <label for="editDate">Дата:</label>
          <input type="date" id="editDate" v-model="currentTag.date" />
        </div>
        <button @click="saveChanges">Сохранить изменения</button>
        <button @click="closeEditModal">Закрыть</button>
      </div>
    </div>

    <button @click="printAllTags">Печать всех ценников</button>
  </div>
</template>

<script setup>
import { ref } from "vue";

// Функция для получения текущей даты в формате 'YYYY-MM-DD'
const getCurrentDate = () => {
  const today = new Date();
  return today.toISOString().split("T")[0]; // Формат YYYY-MM-DD
};

const priceTags = ref([
  {
    productName: "Товар 1",
    oldPrice: 120,
    productPrice: 100,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 2",
    oldPrice: 180,
    productPrice: 150,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 3",
    oldPrice: 250,
    productPrice: 200,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 4",
    oldPrice: 300,
    productPrice: 250,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 5",
    oldPrice: 400,
    productPrice: 300,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 6",
    oldPrice: 450,
    productPrice: 350,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 7",
    oldPrice: 500,
    productPrice: 400,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 8",
    oldPrice: 600,
    productPrice: 450,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 9",
    oldPrice: 700,
    productPrice: 500,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 10",
    oldPrice: 800,
    productPrice: 550,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 11",
    oldPrice: 900,
    productPrice: 600,
    date: getCurrentDate(),
  },
  {
    productName: "Товар 12",
    oldPrice: 1000,
    productPrice: 650,
    date: getCurrentDate(),
  },
]);

const isEditModalOpen = ref(false);
const currentTag = ref({});
const currentTagIndex = ref(null);

const openEditModal = (tag, index) => {
  currentTag.value = { ...tag }; // создаем копию для редактирования
  currentTagIndex.value = index;
  isEditModalOpen.value = true;
};

const saveChanges = () => {
  priceTags.value[currentTagIndex.value] = currentTag.value;
  closeEditModal();
};

const closeEditModal = () => {
  isEditModalOpen.value = false;
};

const printAllTags = () => {
  const printContent = document.createElement("div");
  priceTags.value.forEach((tag) => {
    printContent.innerHTML += `
      <div class="print-price-tag" style="text-align: center; padding: 20px; border: 2px dashed #000; border-radius: 10px; margin: 10px;">
        <p style="font-size: 12px; margin: 0;">ИП БИРЮКОВА</p>
        <h4 style="font-size: 24px; margin: 0;">${tag.productName}</h4>
        <div style="background-color: red; color: white; padding: 5px; display: inline-block;">АКЦИЯ</div>
        <div style="margin-top: 10px;">
          <span style="text-decoration: line-through; font-size: 20px; margin-right: 10px;">${tag.oldPrice} ₽</span>
          <span style="font-size: 28px; font-weight: bold;">${tag.productPrice} ₽</span>
        </div>
        <p style="font-size: 12px; margin-top: 4px;">${tag.date}</p>
      </div>
    `;
  });

  const newWindow = window.open("", "", "width=800,height=600");
  newWindow.document.write("<html><head><title>Печать ценников</title>");
  newWindow.document.write(
    "<style>body { font-family: Arial, sans-serif; display: flex; flex-wrap: wrap; justify-content: center; }</style>"
  );
  newWindow.document.write("</head><body>");
  newWindow.document.write(printContent.innerHTML);
  newWindow.document.write("</body></html>");
  newWindow.document.close();
  newWindow.print();
};
</script>

<style scoped>
.price-tag-manager {
  max-width: 800px;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.price-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.price-tag {
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 10px;
  flex: 1 1 calc(25% - 10px); /* 4 ценника в ряд */
  text-align: center;
}

.business-name {
  font-size: 12px;
  margin: 0;
}

.promotion-banner {
  background-color: red;
  color: white;
  padding: 5px;
  display: inline-block;
  margin: 10px 0;
}

.prices {
  margin-top: 10px;
}

.old-price {
  text-decoration: line-through;
  color: #999;
  font-size: 20px;
  margin-right: 10px;
}

.new-price {
  font-size: 28px;
  font-weight: bold;
  color: #000;
}

.date {
  font-size: 12px;
  margin-top: 10px;
}

.edit-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.edit-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
}

.input-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
