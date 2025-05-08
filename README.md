### [👉👉👉♥♥点此进入♥观看入口👈👈👈](http://a.d44k.cc/crm.html)
<br></br><br></br><br></br>
def handle_events(self):
        """处理输入事件"""
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                self.running = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_r and self.game_over:
                    self.reset_game()
    
    def update(self):
        """更新游戏状态"""
        keys = pygame.key.get_pressed()
        
        # 更新赛车
        self.car.update(keys, self.track.surface)
        
        # 碰撞检测
        if self.track.check_collisions(self.car):
            self.game_over = True
        else:
            # 简单计分 - 每帧得分（实际应该更精确）
            self.score += 1
            
            # 更新单圈时间（简化版）
            current_time = pygame.time.get_ticks()
            self.lap_time = (current_time - self.start_time) / 1000  # 转换为秒
    
    def render(self):
        """渲染游戏画面"""
        screen.fill(BLACK)
        self.track.draw(screen)
        self.car.draw(screen)
        
        # 显示信息
        score_text = self.font.render(f"分数: {self.score}", True, WHITE)
        lap_time_text = self.font.render(f"时间: {self.lap_time:.2f}秒", True, WHITE)
        
        if self.game_over:
            game_over_text = self.font.render("游戏结束! 按R键重新开始", True, RED)
            screen.blit(game_over_text, (WIDTH//2 - 150, HEIGHT//2))
        else:
            screen.blit(score_text, (10, 10))
            screen.blit(lap_time_text, (10, 50))
        
        pygame.display.flip()
    
    def reset_game(self):
        """重置游戏"""
        self.car = Car(WIDTH//2, HEIGHT//2, "car.png")
        self.track = Track()
        self.game_over = False
        self.score = 0
        self.lap_time = 0
        self.start_time = pygame.time.get_ticks()
 
# 创建并运行游戏
if __name__ == "__main__":
    # 确保有assets目录
    if not os.path.exists("assets"):
        os.makedirs("assets")
    
    # 示例：创建一个简单的赛车图片（如果没有真实图片）
    # 实际应用中应该准备真实的赛车图片
    car_img_path = "assets/car.png"
    if not os.path.exists(car_img_path):
        # 创建一个简单的红色矩形作为赛车
        import numpy as np
        from PIL import Image
        
        car_img = Image.new("RGBA", (200, 100), (255, 0, 0, 0))
        draw = ImageDraw.Draw(car_img)
        draw.rectangle([(20, 20), (180, 80)], fill=(255, 0, 0, 255))
        draw.polygon([(20, 50), (0, 30), (0, 70)], fill=(0, 0, 0, 255))  # 左前轮
        draw.polygon([(180, 50), (200, 30), (200, 70)], fill=(0, 0, 0, 255))  # 右前轮
        car_img.save(car_img_path)
    
    game = RacingGame()
    game.run()
    pygame.quit()
    sys.exit()
