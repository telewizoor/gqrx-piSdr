1. rtl sdr drivers:

sudo apt-get install libusb-1.0-0-dev git cmake pkg-config
git clone https://github.com/rtlsdrblog/rtl-sdr-blog
cd rtl-sdr-blog
mkdir build
cd build
cmake ../ -DINSTALL_UDEV_RULES=ON
make
sudo make install
sudo cp ../rtl-sdr.rules /etc/udev/rules.d/
sudo ldconfig

echo 'blacklist dvb_usb_rtl28xxu' | sudo tee --append /etc/modprobe.d/blacklist-dvb_usb_rtl28xxu.conf

sudo reboot


2. keep drivers:

sudo apt-mark hold rtl-sdr librtlsdr0 librtlsdr-dev

3. gr-osmosdr to update librtlsdr:

sudo apt-get install gnuradio
git clone git://git.osmocom.org/gr-osmosdr
cd gr-osmosdr/
mkdir build
cd build/
cmake ../
make
sudo make install
sudo ldconfig

4. build gqrx:

sudo apt-get install qt5
sudo apt-get install qtbase5-dev
sudo apt-get install qtdeclarative5-dev
sudo apt-get install qtmultimedia5-dev
sudo apt-get install libqt5svg5-dev
sudo apt-get install libpulse-dev
sudo apt-get update
sudo apt-get install -y cmake gnuradio-dev gr-osmosdr qt6-base-dev qt6-svg-dev qt6-wayland libasound2-dev libjack-jackd2-dev portaudio19-dev libpulse-dev
git clone https://github.com/gqrx-sdr/gqrx.git
cd gqrx
mkdir build
cd build
cmake ..
make
sudo make install





Hej, rozwiązałem to. To problem sterowników. Instalacja GQRX aktualizuje niektóre pakiety rtlsdr, zakłócając wcześniej zainstalowany sterownik V4. (Właściwie, bez sterownika odbiera sygnał, ale ma około 40 MHz przesunięcia w dół). Oto kroki, które u mnie zadziałały:

*Zainstaluj sterowniki V4 www.rtl-sdr.com/v4

*Wykonaj tę komendę: sudo apt-mark hold rtl-sdr librtlsdr0 librtlsdr-dev (naprawdę nie wiem, czy te trzy pakiety są konieczne do zatrzymania, ale przynajmniej rtlsdr0 jest kluczowy)

*Zainstaluj gr-osmosdr ze źródeł https://github.com/Nuand/gr-osmosdr (To był pakiet odpowiedzialny za aktualizację rtlsdr0)

*Zainstaluj GQRX ze źródeł https://www.gqrx.dk/download/gqrx-sdr-for-the-raspberry-pi I to wszystko. (Przepraszam za mój nie bardzo dobry angielski).

