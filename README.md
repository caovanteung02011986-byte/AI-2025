# AI-2025# AI-2025
import os
from yt_dlp import YoutubeDL
from moviepy.editor import VideoFileClip, TextClip, CompositeVideoClip

def download_video(url):
    """Tải video từ link đầu vào"""
    options = {'outtmpl': 'input_video.mp4'}
    with YoutubeDL(options) as ydl:
        ydl.download([url])
    return "input_video.mp4"

def split_and_sub(video_path, segment_duration=60):
    """Cắt video và thêm chữ chạy"""
    video = VideoFileClip(video_path)
    total_duration = int(video.duration)
    
    # Chia video thành các đoạn nhỏ
    for i, start_time in enumerate(range(0, total_duration, segment_duration)):
        end_time = min(start_time + segment_duration, total_duration)
        
        # Cắt đoạn video
        clip = video.subclip(start_time, end_time)
        
        # Tạo chữ chạy (Ví dụ đơn giản: hiển thị tiêu đề)
        # Lưu ý: Cần cài đặt ImageMagick để sử dụng TextClip
        txt_clip = TextClip(f"Phần {i+1}", fontsize=70, color='yellow', font='Arial-Bold')
        txt_clip = txt_clip.set_pos('center').set_duration(clip.duration)
        
        # Hợp nhất video và chữ
        final_clip = CompositeVideoClip([clip, txt_clip])
        
        # Xuất file
        final_clip.write_videofile(f"output_part_{i+1}.mp4", codec="libx264")

# Sử dụng thử
# url = "LINK_VIDEO_CUA_BAN"
# video_file = download_video(url)
# split_and_sub(video_file)
