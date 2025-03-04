<template>
  <v-container>
    <v-btn color="primary" @click="exportToPDF">Exportar PDF</v-btn>

    <v-data-table :headers="headers" :items="items" class="mt-4"></v-data-table>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { jsPDF } from "jspdf";
import autoTable from "jspdf-autotable";

export default defineComponent({
  data() {
    // Definição de tipos
    interface Item {
      id: number;
      name: string;
      age: number;
    }

    type Header = { title: string; key: keyof Item }; // Garantir que key seja uma chave de Item

    const headers: Header[] = [
      { title: "ID", key: "id" },
      { title: "Nome", key: "name" },
      { title: "Idade", key: "age" },
    ];

    const items: Item[] = [
      { id: 1, name: "João", age: 25 },
      { id: 2, name: "Maria", age: 30 },
      { id: 3, name: "Carlos", age: 28 },
    ];

    return {
      headers,
      items,
    };
  },
  methods: {
    exportToPDF(): void {
      const doc = new jsPDF();
      doc.text("Relatório de Usuários", 14, 10);

      // Extrair cabeçalhos e dados da v-data-table
      const columns = this.headers.map((header) => header.title);
      const rows = this.items.map((item) =>
        this.headers.map((header) => item[header.key]) // Acesso diretamente com o tipo correto
      );

      autoTable(doc, {
        head: [columns],
        body: rows,
        startY: 20, // Posição inicial da tabela
      });

      doc.save("tabela.pdf");
    },
  },
});


// exportToPDF(): void {
//   const doc = new jsPDF();
//   doc.text("Relatório de Usuários", 14, 10);

//   // Extrair cabeçalhos e dados da v-data-table
//   const columns = this.headers.map((header) => header.title);
//   const rows = this.items.map((item) =>
//     this.headers.map((header) => item[header.key as keyof typeof item]) // Garante que key seja reconhecido
// );
</script>




<style scoped>

</style>