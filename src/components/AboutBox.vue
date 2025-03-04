<template>
  <v-card>
    <v-card-title class="d-flex flex-row justify-center">
      <vol-view-full-logo />
    </v-card-title>
    <v-alert color="secondary" variant="tonal" class="notice">
      For investigational use only
    </v-alert>
    <v-card-text>
      <v-btn
        class="my-2"
        @click="openKeyboardShortcuts"
        prepend-icon="mdi-keyboard"
        color="secondary"
      >
        Keyboard Shortcuts and View Controls
      </v-btn>
      <h2 class="mt-2">About VolView</h2>
      <v-divider class="mb-2" />
      <p class="float-right">
        <v-img
          v-show="!mobile"
          src="../assets/KitwareHeadAndNeck.jpg"
          alt="Head and neck CT rendering"
          width="200px"
          class="ma-1"
          align="center"
        /><br />
      </p>
      <p>
        <a
          rel="noopener noreferrer"
          target="_blank"
          href="https://volview.kitware.com/"
        >
          <span>VolView</span>
        </a>
        is an open-source web application developed at
        <a
          rel="noopener noreferrer"
          target="_blank"
          href="https://kitware.com/"
        >
          <span>Kitware</span>
        </a>
        for visualizing and annotating medical images. It key features include:
      </p>

      <ul>
        <li>Fast: Drag-and-drop DICOM files for quick viewing</li>
        <li>
          Flexible: Designed to be easily integrated into existing systems
        </li>
        <li>
          Beautiful: Cinematic volume rendering to generate high-quality 3D
          visualizations
        </li>
        <li>
          Friendly: Familiar radiological image visualization and annotation
          tools
        </li>
        <li>
          Secure: Your data stays on your machine. No cloud services or data
          servers are used.
        </li>
      </ul>
      <br />
      VolView is freely available for research, educational, and commercial
      applications. It is built on a variety of open-source toolkits created by
      Kitware, such as
      <a
        rel="noopener noreferrer"
        target="_blank"
        href="https://github.com/InsightSoftwareConsortium/itk-wasm/"
      >
        <span>itk-wasm</span>
      </a>
      for DICOM I/O and image processing, and
      <a
        rel="noopener noreferrer"
        target="_blank"
        href="https://github.com/Kitware/vtk-js"
      >
        <span>vtk.js</span>
      </a>
      for in-browser scientific visualization.
      <br />
      <br />
      Want help customizing VolView or creating a new web-based visualization
      application?
      <a
        rel-="noopener noreferrer"
        target="_blank"
        href="https://www.kitware.com/contact/project/"
        >Contact Kitware!</a
      >
      <h2 class="mt-2">Useful Links</h2>
      <v-divider class="mb-2" />
      <ul>
        <li>
          <span>VolView source code repo: </span>
          <a
            rel="noopener noreferrer"
            target="_blank"
            href="https://github.com/Kitware/VolView"
          >
            https://github.com/Kitware/VolView
          </a>
        </li>
        <li>
          <span>Community support forum: </span>
          <a
            rel="noopener noreferrer"
            target="_blank"
            href="https://discourse.vtk.org/c/web/volview/14"
          >
            https://discourse.vtk.org/c/web/volview/14
          </a>
        </li>
        <li>
          <span>File an bug report or feature request: </span>
          <a
            rel="noopener noreferrer"
            target="_blank"
            href="https://github.com/Kitware/VolView/issues"
          >
            https://github.com/Kitware/VolView/issues
          </a>
        </li>
      </ul>
      <h2>Version Info</h2>
      <v-divider class="mb-2" />
      <ul>
        <li>
          <div class="d-flex flex-flow align-center text-no-wrap">
            <span>VolView: </span>
            <v-badge inline :content="versions.volview" />
          </div>
        </li>
        <li>
          <div class="d-flex flex-flow align-center text-no-wrap">
            <span>vtk.js: </span>
            <v-badge inline :content="versions['vtk.js']" />
          </div>
        </li>
        <li>
          <div class="d-flex flex-flow align-center text-no-wrap">
            <span>itk-wasm: </span>
            <v-badge inline :content="versions['itk-wasm']" />
          </div>
        </li>
      </ul>
      <h2 class="mt-2">Acknowledgments</h2>
      <v-divider class="mb-2" />
      This work was funded, in part, by the NIH via NIBIB and NIGMS R01EB021396,
      NIBIB R01EB014955, NCI R01CA220681, and NINDS R42NS086295
      <br />
      <br />
      Sample data provided by the following sources:
      <ul>
        <li>
          PROSTATEx Challenge Data: Geert Litjens, Oscar Debats, Jelle Barentsz,
          Nico Karssemeijer, and Henkjan Huisman. "ProstateX Challenge data",
          The Cancer Imaging Archive (2017). DOI: 10.7937/K9TCIA.2017.MURS5CL
          (<a
            rel="noopener noreferrer"
            target="_blank"
            href="https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=23691656"
            >TCIA</a
          >)
        </li>
        <li>
          MRA Head and Neck: Test case 98890234 from the "Patient Contributed
          Image Repository" (<a
            rel="noopener noreferrer"
            target="_blank"
            href="http://www.pcir.org/researchers/98890234_20030505_MR.html"
            >PCIR.org</a
          >)
        </li>
        <li>
          3D ultrasound of a fetus: Sample data from
          <a
            rel="noopener noreferrer"
            target="_blank"
            href="http://tomovision.com"
            >tomovision.com</a
          >
        </li>
      </ul>
    </v-card-text>
  </v-card>
</template>

<style scoped>
.notice {
  width: fit-content;
  margin: 0 auto;
  text-transform: uppercase;
}
</style>

<script>
/* global __VERSIONS__ */

import { defineComponent } from 'vue';
import { useDisplay } from 'vuetify';
import VolViewFullLogo from './icons/VolViewFullLogo.vue';
import { useKeyboardShortcutsStore } from '../store/keyboard-shortcuts';

export default defineComponent({
  name: 'AboutBox',
  components: {
    VolViewFullLogo,
  },
  setup() {
    const display = useDisplay();

    const keyboardStore = useKeyboardShortcutsStore();
    const openKeyboardShortcuts = () => {
      keyboardStore.settingsOpen = true;
    };

    return {
      openKeyboardShortcuts,
      mobile: display.xs,
      versions: {
        volview: __VERSIONS__.volview,
        'vtk.js': __VERSIONS__['vtk.js'],
        'itk-wasm': __VERSIONS__['itk-wasm'],
      },
    };
  },
});
</script>
