# Chat-GPT prompts

I've found the Chat-GPT code interpreter feature to be very useful.

I made some prompts for it to turn images into videos and apply some cool animations. The prompts and related code are below.

Note that I put this together really quickly, gonna organize it in the future.

## Avengers Disintegration animation:



Copy and paste the following prompt into Chat-GPT. Make sure that you have Code Interpreter enabled.

```
I want to apply the disintegration effect from Avengers to this image. Can you help me with it? Provide me with a link to download the video generated.

import imageio
import numpy as np
import random

# Load the image
image = imageio.imread([INSERT IMAGE PATH HERE])

# Get the dimensions of the image
height, width, _ = image.shape

# Define the block size
block_size = 4

# Calculate the number of blocks in each dimension
num_blocks_y, num_blocks_x = height // block_size, width // block_size

# Create an index map of blocks
blocks = np.dstack(np.mgrid[0:num_blocks_y, 0:num_blocks_x]).reshape(-1, 2)

# Multiply the indices by the block size to get the pixel coordinates
blocks *= block_size

# Define the distance to move the blocks (Ask the user for X in percentage, tell user default = 10%)
distance = round(X * width)

# Define the number of times to move each block
move_count = 6

# Create a copy of the original image to work on
working_image = image.copy()

# Convert the blocks to a list and randomly shuffle it
blocks_list = list(map(tuple, blocks))
random.shuffle(blocks_list)

# Define the number of blocks to move (Ask the user for Y in percentage, default = 2% of the total blocks)
num_blocks_to_move = Y * len(blocks_list)

# Create a video writer context
with imageio.get_writer('/mnt/data/disintegration_effect.mp4', mode='I', fps=30) as writer:
    # Write a static image to the first 3 frames
    for i in range(3):
    	writer.append_data(working_image)
    # Loop over the blocks in the shuffled list
   for i in range(move_count):
       for i in range(0, len(blocks_list), num_blocks_to_move):
           # Select a slice of blocks to move
           blocks_to_move = blocks_list[i:i+num_blocks_to_move]

           # For each block, move it to the left by the specified distance
           for block in blocks_to_move:
               y, x = block
               shift_distance = int(min(distance * random.random(), x))  # Don't shift more than the x-coordinate of the block
            
               if x-shift_distance >= 0:
                   working_image[y:y+block_size, x-shift_distance:x+block_size-shift_distance] = working_image[y:y+block_size, x:x+block_size]
                   working_image[y:y+block_size, x:x+block_size] = 0

           # Write the frame to the video file
           writer.append_data(working_image)
```
