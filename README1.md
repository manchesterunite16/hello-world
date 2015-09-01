import urllib.request
import urllib.parse
import codecs
import os
import re

def url_open(url):
    
    req = urllib.request.Request(url)
    req.add_header('User-Agent', 'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/31.0.1650.63 Safari/537.36')
    response = urllib.request.urlopen(req)
    html = response.read()
    if html[:3] == codecs.BOM:
        html = html[:3]
    
    return html

def find_image(url):
    html = url_open(url).decode('gb2312','ignore')
    p = r'<img src="([\S]*\.jpg)"'
    img_addrs = re.findall(p, html)
    print(img_addrs)
    return img_addrs     

def save_imgs(folder, img_addrs):
    
    
    for each in img_addrs:
        filename = each.split('/')[-1]  # 最后一个名字
        with open(filename, 'wb') as f:
            img = url_open(each)
            f.write(img)

    print('pass')
               
def download_mm(folder = 'image'):
    os.mkdir(folder)         # 创建文件夹用来保存文件
    os.chdir(folder)         # 改变目录地址，使之为当期目录
    for page in range(1,2):
        url = "http://slide.sports.sina.com.cn/g_laliga/slide_2_730_81563.html/d/1#p=" + str(page)
        img_addrs = find_image(url)
        save_imgs(folder, img_addrs)

if __name__ == '__main__':
    download_mm()
        
