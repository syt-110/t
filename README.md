from PIL import Image
import numpy as np
import mido
from mido import MidiFile, MidiTrack, Message

# Load the image
image_path = r"C:\Users\user\Pictures\Screenshots\螢幕擷取畫面 2023-03-05 172614.png"
image = Image.open(image_path)
pixels = np.array(image)
print(image)
# Define MIDI parameters
midi_file = "output.mid"
ticks_per_beat = 480
tempo = 500000

# Create a MIDI track
mid = MidiFile()
track = MidiTrack()
mid.tracks.append(track)
#track.append(Message('set_tempo', tempo=tempo, time=0))

# Convert pixel values to MIDI notes and add to the track
for pixel_row in pixels:
    for pixel in pixel_row:
        pixel_value = int(np.mean(pixel))
        note = int(np.interp(pixel_value, [0, 255], [0, 127]))
        track.append(Message('note_on', note=note, velocity=64, time=0))
        track.append(Message('note_off', note=note, velocity=64, time=1))

# Save the MIDI file
mid.save(midi_file)
