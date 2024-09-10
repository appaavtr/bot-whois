from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes
import requests
from bs4 import BeautifulSoup

# Fungsi untuk mendapatkan informasi WHOIS dari whois.com
async def whois_info(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    domain = ' '.join(context.args)
    if not domain:
        await update.message.reply_text("Harap berikan nama domain setelah perintah /whois.")
        return

    try:
        # URL WHOIS dengan domain yang diberikan
        url = f"https://www.whois.com/whois/{domain}"
        response = requests.get(url)
        soup = BeautifulSoup(response.text, 'html.parser')

        # Mencari elemen <pre> dengan kelas 'df-raw' untuk data mentah WHOIS
        whois_raw = soup.find('pre', class_='df-raw')
        
        if not whois_raw:
            await update.message.reply_text("Tidak dapat menemukan informasi WHOIS untuk domain ini.")
            return

        # Mengambil teks dari elemen <pre>
        whois_data = whois_raw.get_text(strip=True)

        # Format teks menjadi lebih mudah dibaca
        response_text = "Informasi WHOIS:\n" + whois_data

        # Kirim hasil ke pengguna
        await update.message.reply_text(response_text if response_text else "Informasi tidak ditemukan.")

    except Exception as e:
        await update.message.reply_text(f"Terjadi kesalahan: {e}")

def main() -> None:
    # Token dari BotFather
    application = Application.builder().token("7015019173:AAHWOhT_pb3sFeV-wMLp33ECB4zJmV1n818").build()

    # Handler untuk perintah /whois
    application.add_handler(CommandHandler("whois", whois_info))

    # Mulai bot
    application.run_polling()

if __name__ == '__main__':
    main()
