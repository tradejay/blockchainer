import time
from selenium import webdriver
from PIL import Image
from urllib.parse import urlparse
from PIL import Image
from io import BytesIO
from screenbot import send_photo 




# Your URLs list
urls = [
    "https://polygonscan.com/chart/tx",
    "https://tronscan.org/#/data/charts/txn/daily-txn",
    "https://analytics.solscan.io/overview",
    "https://chiliscan.com/chart?id=tx&metrics=chain%3A88888%3ATransactions%3A%2327aeef%3Aday%3Alinear%3Aspline%3Avisible%3Ay1&zoom=1675479168000-1709672832000",
    "https://etherscan.io/chart/tx"
]

# Define cursor positions for each site with URL as the key
sites_info = {
    urls[0]: ("640", "360", "1560", "960"),
    urls[1]: ("730", "330", "2110", "950"),
    urls[2]: ("680", "560", "1800", "950"),
    urls[3]: ("750", "330", "2110", "960"),
    urls[4]: ("630", "400", "1600", "960")
}


def get_domain_name(url):
    parsed_url = urlparse(url)
    domain_name = parsed_url.netloc  # This gives the domain name part of the URL
    return domain_name

# Function to take and crop screenshots
def take_screenshot_and_crop(name, url, cursor_position):
    global cropped_screenshot_path
    # Assuming the page is already loaded, no need to navigate with driver.get(url)
    time.sleep(3)  # Wait a bit for the page to be fully rendered if needed

    # Take a full-page screenshot and store it in memory
    screenshot_as_png = driver.get_screenshot_as_png()  # Get the screenshot as a PNG in memory
    screenshot = Image.open(BytesIO(screenshot_as_png))  # Open the screenshot image in memory using Pillow

    # Crop the screenshot based on cursor_position
    left, top, right, bottom = map(int, cursor_position)
    cropped_image = screenshot.crop((left, top, right, bottom))  # Crop the image to the desired area

    # Define paths for saving the cropped screenshot
    cropped_screenshot_path = f"screenshots/{name}_cropped.png"
    cropped_image.save(cropped_screenshot_path)  # Save the cropped screenshot to disk

    print(f"Cropped screenshot saved for {name}")

# Iterate through each existing tab
for tab in driver.window_handles:
    driver.switch_to.window(tab)  # Switch to the tab
    current_url = driver.current_url  # Get the current URL

    driver.refresh()  # Refresh the current page
    print(f"Page refreshed for: {current_url}")  # Optionally print a message indicating the page has been refreshed
    time.sleep(5)  # Wait for 5 seconds after refreshing

    # Match the current URL with the one in sites_info dictionary
    for url, cursor_position in sites_info.items():
        if url == current_url:
            name = get_domain_name(url)  # 'tronscan.org'
  # Use the last part of the URL as the name
            take_screenshot_and_crop(name, url, cursor_position)  # Take and crop screenshot
            send_photo(cropped_screenshot_path)
            break  # Break the loop once the matching site is found and processed
