
# Update Video File Timestamps Script for insta 360

This script updates both the **creation date** and **modification date** of video files based on their filenames. It is specifically designed for macOS and works with filenames in the following format from Insta 360:

```
VID_YYYYMMDD_HHMMSS_XX_XXX.ext
```

Where:
- `YYYYMMDD` is the date in the format `Year-Month-Day`
- `HHMMSS` is the time in the format `Hour-Minute-Second`

The script uses `SetFile` to change the **creation date** and `touch` to change the **modification date**.

## Prerequisites

1. **macOS**: The script relies on macOS-specific tools.
2. **Xcode Command Line Tools**: The script uses `SetFile` to update the creation date, which is part of the Xcode Command Line Tools. Install them by running:
   ```bash
   xcode-select --install
   ```

## How to Use

1. **Clone or Download** this repository to your local machine.

2. **Open Terminal** and navigate to the folder containing the script and your video files:
   ```bash
   cd /path/to/your/video/files
   ```

3. **Run the Script** by pasting the following command into your terminal:
   ```bash
   for file in ./VID_*.*
   do
     filename=$(basename "$file")
     date_part=$(echo "$filename" | cut -d'_' -f2)
     time_part=$(echo "$filename" | cut -d'_' -f3)
     new_date_creation="${date_part:4:2}/${date_part:6:2}/${date_part:0:4} ${time_part:0:2}:${time_part:2:2}:${time_part:4:2}"
     new_date_modification="${date_part}${time_part:0:4}.${time_part:4:2}"
     SetFile -d "$new_date_creation" "$file"
     touch -t "$new_date_modification" "$file"
     echo "Updated creation and modification date for: $file"
   done
   ```

4. **Confirm** the changes. The script will output the filenames and the corresponding updated creation and modification dates for each file.

## Example

Given a video file named `VID_20180106_003559_00_006.mp4`, the script will:
- Update the creation date to `01/06/2018 00:35:59`.
- Update the modification date to `01/06/2018 00:35:59`.

## Notes

- The script assumes the filenames are in the format `VID_YYYYMMDD_HHMMSS_XX_XXX.ext`. If your filenames follow a different format, adjustments to the script may be necessary.
- The `SetFile` command updates the creation date, but this command is only available on macOS after installing the Xcode Command Line Tools.

## License

This project is licensed under the MIT License.
