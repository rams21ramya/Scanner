from PIL import Image, ImageDraw
import os

def make_circle_image(input_path, output_path):
    """Convert an image to a circular format and save it"""
    img = Image.open(input_path).convert("RGBA")
    size = min(img.size)

    # Create circular mask
    mask = Image.new('L', (size, size), 0)
    draw = ImageDraw.Draw(mask)
    draw.ellipse((0, 0, size, size), fill=255)

    # Apply mask to image
    output = Image.new('RGBA', (size, size))
    output.paste(img, ((size - img.width) // 2, (size - img.height) // 2))
    output.putalpha(mask)

    output.save(output_path)
    print(f"Circular image saved to: {output_path}")

# Create directory and sample image if they don't exist
input_path = "receipts/sample_receipt.jpg"
output_path = "receipts/circle_receipt.png"

if not os.path.exists(input_path):
    os.makedirs("receipts", exist_ok=True)

    # Generate placeholder image
    img = Image.new("RGB", (400, 400), "white")
    draw = ImageDraw.Draw(img)
    draw.text((50, 180), "Sample Receipt", fill="black")
    img.save(input_path)
    print(f"Created placeholder image at: {input_path}")

# Process the image
make_circle_image(input_path, output_path)