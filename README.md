#!/usr/bin/env python3
import subprocess
import re

def disable_unused_interfaces():
    """
    Script untuk menonaktifkan interface jaringan yang tidak digunakan
    Kriteria: Interface dalam state DOWN dan tidak memiliki IP address
    """
    print("=== Script Penonaktifan Interface Jaringan yang Tidak Digunakan ===\n")
    
    try:
        # Dapatkan daftar interface jaringan
        print("1. Mendapatkan daftar interface jaringan...")
        result = subprocess.run(['ip', 'link', 'show'], capture_output=True, text=True)
        interfaces = []
        
        # Parse output untuk mendapatkan nama interface
        for line in result.stdout.split('\n'):
            if ':' in line and not line.startswith(' '):
                interface = line.split(':')[1].strip()
                if interface and not interface.startswith('lo'):  # Skip loopback
                    interfaces.append(interface)
        
        print(f"Interface yang terdeteksi: {interfaces}")
        
        # Cek status masing-masing interface
        print("\n2. Memeriksa status setiap interface...")
        disabled_count = 0
        
        for interface in interfaces:
            print(f"\n--- Memeriksa interface: {interface} ---")
            
            # Cek state interface (UP/DOWN)
            state_result = subprocess.run(['ip', 'link', 'show', interface], 
                                        capture_output=True, text=True)
            
            # Cek jika interface memiliki IP address
            ip_result = subprocess.run(['ip', 'addr', 'show', interface], 
                                     capture_output=True, text=True)
            
            # Tampilkan informasi interface
            if 'state UP' in state_result.stdout:
                print(f"   Status: UP")
            else:
                print(f"   Status: DOWN")
                
            if 'inet ' in ip_result.stdout:
                print(f"   Memiliki IP address: Ya")
                # Ekstrak IP address
                ip_match = re.search(r'inet (\d+\.\d+\.\d+\.\d+)/', ip_result.stdout)
                if ip_match:
                    print(f"   IP Address: {ip_match.group(1)}")
            else:
                print(f"   Memiliki IP address: Tidak")
            
            # Kriteria untuk menonaktifkan: state DOWN dan tidak memiliki IP
            if 'state DOWN' in state_result.stdout and 'inet ' not in ip_result.stdout:
                print(f"   ⚠️  Interface {interface} memenuhi kriteria untuk dinonaktifkan")
                
                # Konfirmasi (opsional, bisa dihapus untuk otomatis)
                confirm = input(f"   Apakah Anda yakin ingin menonaktifkan {interface}? (y/N): ")
                
                if confirm.lower() == 'y':
                    print(f"   🔴 Menonaktifkan interface {interface}...")
                    disable_result = subprocess.run(['sudo', 'ip', 'link', 'set', interface, 'down'], 
                                                  capture_output=True, text=True)
                    
                    if disable_result.returncode == 0:
                        print(f"   ✅ Interface {interface} berhasil dinonaktifkan")
                        disabled_count += 1
                    else:
                        print(f"   ❌ Gagal menonaktifkan {interface}: {disable_result.stderr}")
                else:
                    print(f"   ✅ Interface {interface} dibiarkan aktif")
            else:
                print(f"   ✅ Interface {interface} sedang digunakan atau sudah nonaktif")
        
        print(f"\n=== Ringkasan ===")
        print(f"Total interface yang diperiksa: {len(interfaces)}")
        print(f"Interface yang dinonaktifkan: {disabled_count}")
        
    except Exception as e:
        print(f"❌ Error: {e}")
        print("Pastikan Anda menjalankan script dengan hak akses sudo")

if __name__ == "__main__":
    disable_unused_interfaces()
