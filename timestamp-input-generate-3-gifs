# Description: 
    # This script is used to generate 3 optimised gifs (5 sec) from a video.
    # Clip timestamps is determined by user input

# Requirements:
    # Make sure you have ffmpeg and gifsicle installed

# To Run: 
    # 1. Ensure videos are in the same folder as this script
    # 2. bash timestamp-input-generate-3-gifs

# To-do:
    # - Add option to generate n clip(s) instead of 3
    # - Add option to select length of clip (not just 5 seconds)
    # - Add option to generate clips from a single video instead of all videos in a folder
    # - Add error handling for user input (not a number, not valid timestamp)

# ====================================================================================================================
# ============================= Normalise Names
# ====================================================================================================================
printf "START: Normalising names...\n\n"

shopt -s extglob
for i in *.mp4
do
    new_i=$i
    new_i=${new_i// \[*]}
    new_i=${new_i// /_}
    mv "$i" "$new_i"

    printf "Old: \"$i\" \nNew: \"$new_i\"\n\n"
done

printf "FINISH: normalising names!\n"

# ====================================================================================================================
# ============================= Split video into 3 clips, 5 sec each and compress to CRF of 29
# ============================= User input for each video "num1 num2 num3" which is the timestamps to trim from
# ====================================================================================================================
printf "START: splitting videos into 3 and compressing...\n\n"

mkdir all-5-sec-vids

for i in *.mp4; 
do \
    read -p "Enter three timestamps to trim from $i. " time1 time2 time3 
    echo "You have entered: $time1 $time2 $time3" \
    
    ffmpeg -i "$i" \
	-ss "$time1" -t 5 -fs 1M -crf 29 "all-5-sec-vids/$i-1.mp4" \
	-ss "$time2" -t 5 -fs 1M -crf 29 "all-5-sec-vids/$i-2.mp4" \
	-ss "$time3" -t 5 -fs 1M -crf 29 "all-5-sec-vids/$i-3.mp4"
done

printf "FINISH: splitting videos into 3 clips and compressing!\n"

# ====================================================================================================================
# ============================= Convert split vids into gifs
# ====================================================================================================================
printf "START: converting split videos into gifs...\n\n"

cd all-5-sec-vids
mkdir all-5-sec-gifs

for file in *.mp4; 

do ffmpeg -y -i "$file" -filter_complex "fps=25,scale=320:-1:flags=lanczos,split[s0][s1];[s0]palettegen=max_colors=72[p];[s1][p]paletteuse" "all-5-sec-gifs/$file".gif \

done

printf "FINISH: converting split videos into gifs!\n"

# ====================================================================================================================
# ============================= Optimise gifs
# ====================================================================================================================
printf "START: optimising gifs...\n\n"

cd all-5-sec-gifs
mkdir all-optimised-gifs

for file in *.gif; 

do gifsicle -O3 "$file" --lossy=200 -o "all-optimised-gifs/$file".gif

done

printf "FINISH: creating clips!\n"