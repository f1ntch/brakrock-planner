<template>
  <div id="app">
    <div class="header">
      <div class="logo-container">
        <img src="/brakrock-logo.png" alt="Brakrock Festival" class="logo" />
      </div>
      <h1>🎸 Brakrock </h1>
      <p>Planner – Saturday, August 2, 2025</p>
    </div>

    <!-- Spotify Playlist Section -->
    <div class="spotify-section">
      <h2 class="spotify-title">🎵 Festival Vibes Playlist</h2>
      <div class="spotify-container">
        <iframe 
          style="border-radius:12px" 
          src="https://open.spotify.com/embed/playlist/6cqFV1jSiUZTPJQlSJd7CM?utm_source=generator" 
          width="100%" 
          height="352" 
          frameBorder="0" 
          allowfullscreen="" 
          allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" 
          loading="lazy">
        </iframe>
      </div>
    </div>

    <div class="lanes-container">
      <!-- Woodstage Lane -->
      <div class="stage-lane">
        <h2 class="lane-title woodstage">🟫 Woodstage</h2>
        <div class="bands-container">
          <div 
            v-for="band in woodstageBands" 
            :key="band.id"
            class="band-item woodstage"
            draggable="true"
            @dragstart="onDragStart($event, band)"
          >
            <div class="band-name">{{ band.name }}</div>
            <div class="band-time">{{ band.time }}</div>
          </div>
        </div>
      </div>

      <!-- Riverstage Lane -->
      <div class="stage-lane">
        <h2 class="lane-title riverstage">🔵 Riverstage</h2>
        <div class="bands-container">
          <div 
            v-for="band in riverstageBands" 
            :key="band.id"
            class="band-item riverstage"
            draggable="true"
            @dragstart="onDragStart($event, band)"
          >
            <div class="band-name">{{ band.name }}</div>
            <div class="band-time">{{ band.time }}</div>
          </div>
        </div>
      </div>

      <!-- Lakestage Lane -->
      <div class="stage-lane">
        <h2 class="lane-title lakestage">🟢 Lakestage</h2>
        <div class="bands-container">
          <div 
            v-for="band in lakestageBands" 
            :key="band.id"
            class="band-item lakestage"
            draggable="true"
            @dragstart="onDragStart($event, band)"
          >
            <div class="band-name">{{ band.name }}</div>
            <div class="band-time">{{ band.time }}</div>
          </div>
        </div>
      </div>

      <!-- Planner Lane -->
      <div class="planner-lane">
        <h2 class="lane-title planner">📋 My Day Planner</h2>
        
        <div class="stats">
          <span>Bands: {{ plannedBands.length }}</span>
          <span v-if="overlaps.length > 0" class="overlap-count">
            ⚠️ {{ overlaps.length }} overlap(s)
          </span>
        </div>

        <div 
          class="planner-container"
          :class="{ 'drag-over': isDragOver }"
          @dragover="onDragOver"
          @dragleave="onDragLeave"
          @drop="onDrop"
        >
          <div v-if="overlaps.length > 0" class="overlap-warning">
            ⚠️ Overlapping performances detected!
          </div>

          <div v-if="plannedBands.length === 0" class="empty-planner">
            <p>🎵 Drag bands here!</p>
          </div>

          <div 
            v-for="(band, index) in plannedBands" 
            :key="band.id"
            class="planner-item"
            :class="[
              band.stage.toLowerCase(),
              { 'overlap': isOverlapping(band) }
            ]"
          >
            <button 
              class="remove-btn" 
              @click="removeBand(index)"
              title="Remove from planner"
            >
              ×
            </button>
            <div class="band-name">{{ band.name }}</div>
            <div class="band-time">{{ band.time }}</div>
            <div class="band-stage">{{ band.stage }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      isDragOver: false,
      plannedBands: [],
      woodstageBands: [
        { id: 'w1', name: 'Corbillard', time: '11:45 – 12:20', stage: 'Woodstage', startTime: '11:45', endTime: '12:20' },
        { id: 'w2', name: 'Smoke or Fire', time: '12:55 – 13:35', stage: 'Woodstage', startTime: '12:55', endTime: '13:35' },
        { id: 'w3', name: 'Ratboy', time: '14:15 – 14:55', stage: 'Woodstage', startTime: '14:15', endTime: '14:55' },
        { id: 'w4', name: 'Gob', time: '15:40 – 16:20', stage: 'Woodstage', startTime: '15:40', endTime: '16:20' },
        { id: 'w5', name: 'Sloppy Seconds', time: '17:05 – 17:50', stage: 'Woodstage', startTime: '17:05', endTime: '17:50' },
        { id: 'w6', name: 'The Locos', time: '18:45 – 19:35', stage: 'Woodstage', startTime: '18:45', endTime: '19:35' },
        { id: 'w7', name: 'Codefendants', time: '20:30 – 21:15', stage: 'Woodstage', startTime: '20:30', endTime: '21:15' },
        { id: 'w8', name: 'Madball', time: '22:05 – 23:00', stage: 'Woodstage', startTime: '22:05', endTime: '23:00' },
        { id: 'w9', name: 'Zeke', time: '00:05 – 01:00', stage: 'Woodstage', startTime: '00:05', endTime: '01:00' }
      ],
      riverstageBands: [
        { id: 'r1', name: 'The Corps', time: '12:20 – 12:55', stage: 'Riverstage', startTime: '12:20', endTime: '12:55' },
        { id: 'r2', name: 'Urethane', time: '13:35 – 14:15', stage: 'Riverstage', startTime: '13:35', endTime: '14:15' },
        { id: 'r3', name: 'Bodyjar', time: '14:55 – 15:40', stage: 'Riverstage', startTime: '14:55', endTime: '15:40' },
        { id: 'r4', name: 'Save Ferris', time: '16:20 – 17:05', stage: 'Riverstage', startTime: '16:20', endTime: '17:05' },
        { id: 'r5', name: 'Nerf Herder', time: '17:50 – 18:45', stage: 'Riverstage', startTime: '17:50', endTime: '18:45' },
        { id: 'r6', name: 'Agnostic Front', time: '19:35 – 20:30', stage: 'Riverstage', startTime: '19:35', endTime: '20:30' },
        { id: 'r7', name: 'Circle Jerks', time: '21:15 – 22:05', stage: 'Riverstage', startTime: '21:15', endTime: '22:05' },
        { id: 'r8', name: 'Millencolin', time: '23:00 – 00:05', stage: 'Riverstage', startTime: '23:00', endTime: '00:05' }
      ],
      lakestageBands: [
        { id: 'l1', name: 'Royking', time: '11:45 – 12:20', stage: 'Lakestage', startTime: '11:45', endTime: '12:20' },
        { id: 'l2', name: 'Wolfpack', time: '12:55 – 13:30', stage: 'Lakestage', startTime: '12:55', endTime: '13:30' },
        { id: 'l3', name: 'Popes of Chillitown', time: '14:00 – 14:40', stage: 'Lakestage', startTime: '14:00', endTime: '14:40' },
        { id: 'l4', name: 'The Anti-Queens', time: '15:15 – 15:55', stage: 'Lakestage', startTime: '15:15', endTime: '15:55' },
        { id: 'l5', name: 'MakeWar', time: '16:25 – 17:10', stage: 'Lakestage', startTime: '16:25', endTime: '17:10' },
        { id: 'l6', name: 'Chaser', time: '19:05 – 19:55', stage: 'Lakestage', startTime: '19:05', endTime: '19:55' },
        { id: 'l7', name: 'Misconduct', time: '20:40 – 21:30', stage: 'Lakestage', startTime: '20:40', endTime: '21:30' },
        { id: 'l8', name: 'The Last Gang', time: '22:10 – 23:00', stage: 'Lakestage', startTime: '22:10', endTime: '23:00' },
        { id: 'l9', name: 'NOFX (The Decline + more)', time: '23:30 – 01:00', stage: 'Lakestage', startTime: '23:30', endTime: '01:00' }
      ]
    }
  },
  computed: {
    overlaps() {
      const overlaps = []
      for (let i = 0; i < this.plannedBands.length; i++) {
        for (let j = i + 1; j < this.plannedBands.length; j++) {
          if (this.doBandsOverlap(this.plannedBands[i], this.plannedBands[j])) {
            overlaps.push([this.plannedBands[i], this.plannedBands[j]])
          }
        }
      }
      return overlaps
    }
  },
  methods: {
    onDragStart(event, band) {
      event.dataTransfer.setData('text/plain', JSON.stringify(band))
    },
    onDragOver(event) {
      event.preventDefault()
      this.isDragOver = true
    },
    onDragLeave(event) {
      this.isDragOver = false
    },
    onDrop(event) {
      event.preventDefault()
      this.isDragOver = false
      
      const bandData = event.dataTransfer.getData('text/plain')
      if (bandData) {
        const band = JSON.parse(bandData)
        // Check if band is already in planner
        if (!this.plannedBands.find(b => b.id === band.id)) {
          this.plannedBands.push(band)
        }
      }
    },
    removeBand(index) {
      this.plannedBands.splice(index, 1)
    },
    parseTime(timeStr) {
      const [hours, minutes] = timeStr.split(':').map(Number)
      return hours * 60 + minutes
    },
    doBandsOverlap(band1, band2) {
      const start1 = this.parseTime(band1.startTime)
      let end1 = this.parseTime(band1.endTime)
      const start2 = this.parseTime(band2.startTime)
      let end2 = this.parseTime(band2.endTime)
      
      // Handle midnight crossover
      if (end1 < start1) end1 += 24 * 60
      if (end2 < start2) end2 += 24 * 60
      
      return start1 < end2 && start2 < end1
    },
    isOverlapping(band) {
      return this.plannedBands.some(otherBand => 
        otherBand.id !== band.id && this.doBandsOverlap(band, otherBand)
      )
    }
  }
}
</script> 