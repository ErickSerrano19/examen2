<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Perfil</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <ion-list>
        <ion-list-header>Sesión</ion-list-header>
        <ion-item>
          <ion-icon slot="start" :icon="personCircleOutline" aria-hidden="true" />
          <ion-label>{{ usuario?.nombre ?? '—' }}</ion-label>
        </ion-item>

        <!--
          TODO [M7] Sección "Dispositivo" con ion-list-header y un ion-item por dato:
            Plataforma (Capacitor.getPlatform()), Modelo, Sistema (operatingSystem + osVersion)
            y Conexión (ion-badge con `tipo` o "sin conexión" según `conectado`).
        -->

        <ion-list-header>Dispositivo</ion-list-header>
        <ion-item>
          <ion-label>Plataforma: {{ plataforma }}</ion-label>
        </ion-item>
        <ion-item>
          <ion-label>Modelo: {{ dispositivo?.model ?? '—' }}</ion-label>
        </ion-item>
        <ion-item>
          <ion-label>Sistema: {{ sistema }}</ion-label>
        </ion-item>
        <ion-item>
          <ion-label>Conexión</ion-label>
          <ion-badge slot="end" :color="conectado ? 'success' : 'danger'">
            {{ conectado ? tipo : 'sin conexión' }}
          </ion-badge>
        </ion-item>
      </ion-list>

      <div class="ion-padding">
        <ion-button expand="block" color="danger" fill="outline" @click="salir">Cerrar sesión</ion-button>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue';
import {
  IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonList, IonListHeader, IonItem, IonIcon,
  IonLabel, IonBadge, IonButton, onIonViewWillEnter, useIonRouter,
} from '@ionic/vue';
import { personCircleOutline } from 'ionicons/icons';
import { Capacitor } from '@capacitor/core';
import { Device, type DeviceInfo } from '@capacitor/device';
import { useAuth } from '@/composables/useAuth';
import { useConexion } from '@/composables/useConexion';

const { usuario, cerrarSesion } = useAuth();
const { conectado, tipo } = useConexion();
const ionRouter = useIonRouter();

const plataforma = ref(Capacitor.getPlatform());
const dispositivo = ref<DeviceInfo | null>(null);
const sistema = computed(() => {
  const info = dispositivo.value;
  if (!info) return '—';
  return [info.operatingSystem, info.osVersion].filter(Boolean).join(' ');
});

onIonViewWillEnter(async () => {
  plataforma.value = Capacitor.getPlatform();
  dispositivo.value = await Device.getInfo();
});

async function salir() {
  await cerrarSesion();
  ionRouter.navigate('/login', 'root', 'replace');
}
</script>
