# arcade-game
import arcade
import time
import os
import sys
import random
place = 'Easy Jumping'
life = 30
times = 0
all_time = 0
class Life(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237,189,101))
        self.next_level = False
        self.wrong = False
        self.camera = arcade.camera.Camera2D()
    def on_draw(self):
        global life
        self.clear()
        self.camera.use()
        arcade.draw_lrbt_rectangle_filled(200,600,280,320,(255,255,255))
        arcade.draw_text("how many lifes do you want?           type",200,450,arcade.color.BLACK,15,2)
        arcade.draw_text(life,210,290,arcade.color.BLACK,20,2)
        arcade.draw_text("press Enter to start next level",200,100,arcade.color.BLACK,15,2)
        arcade.draw_text("press any key before you input numbers",400,150,arcade.color.BLACK,15,2)
        if self.wrong == True:
            arcade.draw_text("life can't be 0",200,200,arcade.color.RED,15,2)
    def on_update(self,delta_time):
        global place


        self.camera.position = 400,300
        self.window.set_fullscreen()
        self.camera.viewport = self.window.rect
        self.camera.projection = arcade.LRBT(-400, 400, -300, 300)

        if self.next_level == True and life!=0:
            if place == 'Easy Jumping':
                game1 = Game1()
                self.window.show_view(game1)
            elif place == 'Difficult Jumping':
                game6 = Game6()
                self.window.show_view(game6)
                game6.setup()
        else:
            self.wrong = True


    def on_key_press(self, key, modifiers):
        global  life
        self.wrong = False
        if len(str(life))<10:
            if key == arcade.key.KEY_1:
                life = life * 10 +1
            elif key == arcade.key.KEY_2:
                life = life*10 + 2
            elif key == arcade.key.KEY_3:
                life = life*10 + 3
            elif key == arcade.key.KEY_4:
                life = life*10 + 4
            elif key == arcade.key.KEY_5:
                life = life*10 + 5
            elif key == arcade.key.KEY_6:
                life = life*10 + 6
            elif key == arcade.key.KEY_7:
                life = life*10 + 7
            elif key == arcade.key.KEY_8:
                life = life*10 + 8
            elif key == arcade.key.KEY_9:
                life = life*10 + 9
            elif key == arcade.key.KEY_0:
                life = life*10
        if key == arcade.key.BACKSPACE:
            life_str = str(life)
            life_str_end=life_str[-1]
            life-=int(life_str_end)
            life_str1 = str(life).split('.')

            life_str2 = life_str1[0]
            life = int(life_str2)
            life = life/10
            life = int(life)
        if key == arcade.key.ENTER and life != 0:
            self.next_level = True

class Died(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
    def on_draw(self):
        self.clear()
        arcade.draw_text("you died", 200, 550, arcade.color.BLACK, 50, 4)
        arcade.draw_text("press c to start again", 100, 450, arcade.color.BLACK, 50, 4)
        arcade.draw_text("press q to quit", 200, 350, arcade.color.BLACK, 50, 4)
    def on_key_press(self,key,modifiers):
        global place
        if key == arcade.key.C:
            if place == 'Easy Jumping':
                life = Life()
                self.window.show_view(life)
        if key == arcade.key.Q:
            arcade.close_window()



class Game1(arcade.View):

    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        global times
        self.x = 100
        self.y = 300
        self.left = False
        self.right = False
        self.jump = False
        self.num = 0
        self.little = 0
        time = 0
        #self.camera = arcade.camera.Camera2D()

    def on_draw(self):
        global times
        global all_time
        self.clear()
        #self.camera.use()
        arcade.draw_lrbt_rectangle_filled(0,800,450,600,(214,140,20))
        arcade.draw_lrbt_rectangle_filled(0,800,0,300,(214,140,20))
        arcade.draw_lrbt_rectangle_filled(650,665,300,330,(255,255,215))
        arcade.draw_lrbt_rectangle_filled(self.x,self.x+10,self.y,self.y+20,(50,50,10))
        arcade.draw_text(f'Level:{place}-1',400,550,arcade.color.BLACK,15,2)
        arcade.draw_text(f'time:{times}',250,550,arcade.color.BLACK,15,2)
        arcade.draw_text(f'time in all level:{all_time}',0,0,arcade.color.BLACK,15,2)
        if self.left == True and self.x>0:
            self.x-=5
        if self.right == True and self.x<790:
            self.x+=5
        if self.jump == True:
            self.y+=5
            self.num+=1
            if self.num == 8:
                self.jump = False
                self.num  = 0



    def on_update(self,delta_time):
        global times
        global all_time
        self.little+=1
        if self.little%60 == 0 and self.little!=0:
            times+=1
            all_time+=1
        '''self.camera.viewport = self.window.rect
        self.camera.projection = arcade.LRBT(-400, 400, -300, 300)'''

        if self.y>300 and self.jump == False:
            self.y-=2.5
        if self.x>650 and self.y<310:
            time.sleep(2)
            game2 = Game2()
            self.window.show_view(game2)

    def on_key_press(self,key,modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        elif key == arcade.key.UP and self.y == 300:
            self.jump = True
        elif self.y == 300:
            if key == arcade.key.W or arcade.key.SPACE:
                self.jump = True
    def on_key_release(self,key,modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False

class Game2(arcade.View):

    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.x = 50
        self.y = 300
        self.left = False
        self.right = False
        self.jump = False
        self.num = 0
        self.color_trap = (214,140,20)
        self.color_earn_life = (237,189,101)
        self.allow_jump = True
        self.first_time = True
        self.drop_speed = 2
        self.timemove = 0
        #self.camera = arcade.camera.Camera2D()


    def on_draw(self):
        global times
        global all_time
        if self.timemove == 0:
            times = 0
        global life
        self.clear()
        self.timemove+=1
        if self.timemove % 60 == 0 and self.timemove!=0:
            times+=1
            all_time+=1
        #self.camera.use()

        arcade.draw_lrbt_rectangle_filled(0, 800, 450, 600, (214, 140, 20))
        arcade.draw_lrbt_rectangle_filled(0, 100, 0, 300, (214, 140, 20))
        arcade.draw_lrbt_rectangle_filled(650,800,0,300,(214,140,20))
        arcade.draw_lrbt_rectangle_filled(125,250,0,300,(214,140,20))
        arcade.draw_lrbt_rectangle_filled(275,625,0,300,(214,140,20))
        arcade.draw_lrbt_rectangle_filled(625,650,0,300,self.color_trap)
        arcade.draw_lrbt_rectangle_filled(650, 665, 300, 330, (255, 255, 215))
        arcade.draw_lrbt_rectangle_filled(775,800,350,375,self.color_earn_life)
        arcade.draw_lrbt_rectangle_filled(self.x, self.x + 10, self.y, self.y + 20, (50, 50, 10))
        arcade.draw_text(f"life = {life}", 100, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-2',400,550,arcade.color.BLACK,15,2)
        arcade.draw_text(f'time:{times}', 250, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}', 0, 0, arcade.color.BLACK, 15, 2)
        if self.left == True and self.x > 0:
            if self.y>=300:
                self.x -= 5
            elif self.y<300:
                if self.x>100:
                    self.x-=0.4
                elif self.x>250:
                    self.x-=0.4
                elif self.x>625:
                    self.x-=0.4
        if self.right == True and self.x < 790:
            if self.y>=300:
                self.x += 5
            elif self.y<300:
                if self.x<115:
                    self.x+=0.1
                elif self.x<265:
                    self.x+=0.1
                elif self.x<640:
                    self.x+=0.1

        if self.jump == True and self.allow_jump == True:
            self.y += 5
            self.num += 1
            if self.num == 8:
                self.jump = False
                self.num = 0



    def on_update(self, delta_time):
        global life

        if self.y > 300 and self.jump == False:
            self.y -= 2.5
        if self.y == 300 and self.x>100 and self.x<115:
            self.allow_jump = False
        if self.y == 300 and self.x>250 and self.x<265:
            self.allow_jump = False
        if self.y == 300 and self.x>625 and self.x<640:
            self.allow_jump = False
            self.color_trap = (237, 189, 101)
        if self.x>775 and self.y>330 and self.first_time == True:
            self.color_earn_life = (255,215,0)
            life+=1
            self.first_time = False
        if self.allow_jump ==False:
            self.y-=self.drop_speed
            self.drop_speed+=0.2
            time.sleep(0.01)
        if self.y<-100:
            if life>1:
                time.sleep(2)
                self.x = 50
                self.y = 300
                life-=1
                self.color_trap = (214,140,20)
                self.color_earn_life = self.color_earn_life = (237,189,101)
                self.allow_jump = True
                self.drop_speed = 2
            else:
                time.sleep(2)
                dead = Died()
                self.window.show_view(dead)

        if self.x > 650 and self.y < 310 and self.x<665:
            time.sleep(2)
            game3 = Game3()
            self.window.show_view(game3)
            game3.setup()


    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        elif key == arcade.key.UP and self.y == 300:
            self.jump = True
        elif self.y == 300:
            if key == arcade.key.W or arcade.key.SPACE:
                self.jump = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False

class Game3(arcade.View):

    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.player_sprite = None
        self.player_list = None
        self.platform1 = None
        self.platform2 = None
        self.platform_list = None
        self.physics_engine = None
        self.left = False
        self.right = False
        self.jump = False
        self.num = 0
        self.engine = None
        self.wall1 = None
        self.wall_list = None
        self.wall2 = None
        self.wall3 = None
        self.wall4 = None
        self.move1 = False
        self.move2 = False
        self.move3 = False
        self.move4 = False
        self.if_place = 0
        self.move5 = False
        self.wall6 = None
        self.wall7 = None
        self.wall8 = None
        self.wall9 = None
        self.wall10 = None
        self.wall11 = None
        self.wall12 = None
        self.wall13 = None
        self.wall14 = None
        self.wall15 = None
        self.wall16 = None
        self.movetime = 0
        self.first_time = True
        self.wall_rise_count = 0
        self.wall_rise = True
        self.color_trap = (214,140,20)
        self.move1_wall = True
        self.move2_wall = False
        self.move3_wall = False
        self.move4_wall = False
        self.move5_wall = False
        self.move6_wall = False
        self.move7_wall = False
        self.move8_wall = False
        self.move9_wall = False
        self.move10_wall = False
        self.place = 0
        self.change = 0
        self.dead = False
        self.dead_x = 0
        self.dead_y = 0
        #self.camera = arcade.camera.Camera2D()
    def setup(self):
        self.player_list = arcade.SpriteList()
        self.platform_list = arcade.SpriteList()
        self.wall_list = arcade.SpriteList()
        self.piece_list = arcade.SpriteList()
        self.player_sprite = arcade.Sprite("屏幕截图 2025-06-12 115306.png")
        self.player_sprite.center_x = 55
        self.player_sprite.center_y = 200
        self.player_list.append(self.player_sprite)
        self.platform1 = arcade.Sprite("屏幕截图 2025-06-13 160046.png")
        self.platform1.center_x = 130
        self.platform1.center_y = 115
        self.wall_list.append(self.platform1)
        self.platform2 = arcade.Sprite("屏幕截图 2025-06-12 213756.png")
        self.platform2.center_x = 814
        self.platform2.center_y = 102
        self.wall_list.append(self.platform2)
        self.wall1 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall1.center_x = 80
        self.wall1.center_y = 300
        self.wall_list.append(self.wall1)
        self.wall2 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall2.center_x = 156
        self.wall2.center_y = 330
        self.wall_list.append(self.wall2)
        self.wall3 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall3.center_x = 156
        self.wall3.center_y = 300
        self.wall_list.append(self.wall3)
        self.wall4 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall4.center_x = 204
        self.wall4.center_y = 330
        self.wall_list.append(self.wall4)
        self.wall6 = arcade.Sprite("屏幕截图 2025-06-13 183516.png")
        self.wall6.center_x = 429
        self.wall6.center_y = -60
        self.wall_list.append(self.wall6)
        self.wall7 = arcade.Sprite("屏幕截图 2025-06-13 195840.png")
        self.wall7.center_x = 450
        self.wall7.center_y = 241
        self.wall_list.append(self.wall7)
        self.wall8 = arcade.Sprite("屏幕截图 2025-06-13 195840.png")
        self.wall8.center_x = 584
        self.wall8.center_y = 241
        self.wall_list.append(self.wall8)
        self.wall9 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall9.center_x = 820
        self.wall9.center_y = 300
        self.wall_list.append(self.wall9)
        self.wall10 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall10.center_x = 820
        self.wall10.center_y = 300
        self.wall_list.append(self.wall10)
        self.wall11 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall11.center_x = 820
        self.wall11.center_y = 300
        self.wall_list.append(self.wall11)
        self.wall12 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall12.center_x = 820
        self.wall12.center_y = 300
        self.wall_list.append(self.wall12)
        self.wall13 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall13.center_x = 820
        self.wall13.center_y = 300
        self.wall_list.append(self.wall13)
        self.wall14 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall14.center_x = 820
        self.wall14.center_y = 300
        self.wall_list.append(self.wall14)
        self.wall15 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall15.center_x = 820
        self.wall15.center_y = 300
        self.wall_list.append(self.wall15)
        self.wall16 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall16.center_x = 820
        self.wall16.center_y = 300
        self.wall_list.append(self.wall16)
        self.engine = arcade.PhysicsEngineSimple(self.player_sprite,self.wall_list)
        '''for p in range(5):
            for t in range(9):
                self.piece = arcade.Sprite("屏幕截图 2025-06-12 123429.png")
                self.piece.center_x = p*2+300
                self.piece.center_y = t*2+400
                self.piece_list.append(self.piece)
        self.physics_e = arcade.PymunkPhysicsEngine(gravity=(0,-500))
        self.physics_e.add_sprite_list(self.piece_list,elasticity=2,mass=1,body_type=arcade.PymunkPhysicsEngine.DYNAMIC)'''

        #self.draw_broken = False
        self.physics_engine = arcade.PhysicsEnginePlatformer(self.player_sprite, self.platform_list,gravity_constant = 0.5)
        '''self.physics_e.add_sprite_list(self.platform_list,body_type=arcade.PymunkPhysicsEngine.STATIC)
        self.physics_e.add_sprite_list(self.wall_list,body_type=arcade.PymunkPhysicsEngine.STATIC)
        self.death_count = 0
    def arrange_pieces_on_player(self):
        cols = 5
        rows = 9
        spacing = 2
        total_width = (cols - 1) * spacing
        total_height = (rows - 1) * spacing
        player_cx = self.player_sprite.center_x
        player_cy = self.player_sprite.center_y
        start_x = player_cx - total_width / 2
        start_y = player_cy - total_height / 2
        for mmmm in self.piece_list:
            mmmm.remove_from_sprite_lists()
        for p in range(cols):
            for t in range(rows):
                piece = arcade.Sprite("屏幕截图 2025-06-12 123429.png")
                piece.center_x = start_x + p * spacing
                piece.center_y = start_y + t * spacing
                self.piece_list.append(piece)
        self.physics_e = arcade.PymunkPhysicsEngine(gravity=(0,-200))
        self.physics_e.add_sprite_list(self.platform_list,body_type=arcade.PymunkPhysicsEngine.STATIC)
        self.physics_e.add_sprite_list(self.wall_list,body_type=arcade.PymunkPhysicsEngine.STATIC)
        self.physics_e.add_sprite_list(self.piece_list,elasticity=2,mass=1,body_type=arcade.PymunkPhysicsEngine.DYNAMIC)'''
    def on_draw(self):
        global  life
        global times
        global all_time
        if self.movetime == 0:
            times = 0
        self.movetime+=1
        if self.movetime%60 == 0 and self.movetime!=0:
            times+=1
            all_time+=1
        #self.camera.use()
        self.clear()
        arcade.draw_lrbt_rectangle_filled(286,334.5,0,290,self.color_trap)
        '''if self.dead:
            self.piece_list.draw()'''
        self.wall_list.draw()
        self.platform_list.draw()
        arcade.draw_text(f"life = {life}", 100, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-3',400,550,arcade.color.BLACK,15,2)
        arcade.draw_lrbt_rectangle_filled(785,800,290,320,(255,255,215))
        arcade.draw_text(f'time:{times}', 250, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}', 0, 0, arcade.color.BLACK, 15, 2)
        #if not self.dead:
        self.player_list.draw()
    def on_update(self,delta_time):
        global life
        '''if self.dead:
            self.physics_e.step()
        if not self.dead:
            self.arrange_pieces_on_player()
        if self.dead:
            self.death_count+=1
        if self.death_count == 1:
            if not self.draw_broken:
                for xyz in self.piece_list:
                    if xyz.center_y > self.dead_y:
                        self.physics_e.apply_force(xyz, (0, -500))
                    elif xyz.center_y < self.dead_y:
                        self.physics_e.apply_force(xyz, (0, 500))
                    if xyz.center_x > self.dead_x:
                        self.physics_e.apply_force(xyz, (-500, 0))
                    elif xyz.center_x < self.dead_x:
                        self.physics_e.apply_force(xyz, (500, 0))
            #self.draw_broken = True
        '''
        self.physics_engine.update()
        self.engine.update()
        #self.window.set_fullscreen()
        #self.camera.viewport = self.window.rect
        #self.camera.projection = arcade.LRBT(-400, 400, -300, 300)
        #if not self.dead:
        if self.left == True and self.player_sprite.center_x>5:
            #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-07-03 174600.png")
            self.player_sprite.center_x-=7
            self.change+=1
            #if self.change == 1:
                #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-06-30 222800.png")
        if self.right == True and self.player_sprite.center_x<795:
            #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-07-03 174600.png")
            self.player_sprite.center_x+=7
            self.change+=1
            if self.change == 1:
                self.change = 0
                #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-06-30 222800.png")
        if self.jump == True:
            #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-07-03 174600.png")
            self.player_sprite.center_y+=8
            self.num+=1
            if self.num == 8:
                self.num = 0
                #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-06-30 222800.png")
                self.jump = False
        if self.player_sprite.center_x > 115:
            self.if_place+=1
            self.move1 = True
            if self.if_place  == 1 or self.move1 == True and self.move2 == False:
                if self.wall4.center_y<360:
                    self.wall4.center_y+=2
                    if self.wall4.center_y>=360:
                        self.move2 = True
        if self.move2 == True and self.wall4.center_x>108:
            self.wall4.center_x-=5
        if self.wall4.center_x<=108:
            self.move3 = True
        if self.move3 == True and self.wall4.center_y>328:
            self.wall4.center_y-=5
        if self.move3 == True and self.wall4.center_y<=330:
            self.move4 = True
        if self.move4 == True and self.wall4.center_x>80:
            self.wall4.center_x-=5
        if self.move4 == True and self.wall4.center_x<=80:
            self.move5 = True
        if self.move5 == True:
            self.wall4.center_x-=0.5
            self.wall1.center_x-=0.5
        if self.player_sprite.center_x>291 and self.player_sprite.center_x<334:
            if self.player_sprite.center_y <=300:
                self.color_trap = (237, 189, 101)
        if self.player_sprite.center_x>470 and self.move1_wall == True and self.first_time == True:

            self.wall8.center_y+=5
            self.wall7.center_y+=5
            self.wall_rise_count+=1
            if self.wall_rise_count == 19:
                self.wall_rise_count = 0
                self.move1_wall = False
                self.move2_wall = True
        if self.move2_wall == True and self.first_time == True:
            self.wall8.center_x+=1
            self.wall7.center_x+=1
            self.wall_rise_count +=1
            if self.wall_rise_count == 59:
                self.wall_rise_count = 0
                self.move2_wall = False
                self.move3_wall = True
        if self.move3_wall == True and self.first_time == True:
            self.wall8.center_x -= 1
            self.wall7.center_x -= 1
            self.wall_rise_count += 1
            if self.wall_rise_count == 59:
                self.wall_rise_count = 0
                self.move3_wall = False
                self.move4_wall = True
        if self.move4_wall == True and self.first_time == True:
            self.wall8.center_x += 1
            self.wall7.center_x += 1
            self.wall_rise_count += 1
            if self.wall_rise_count == 59:
                self.wall_rise_count = 0
                self.move4_wall = False
                self.move5_wall = True
        if self.move5_wall == True and self.first_time == True:
            self.wall8.center_x -= 1
            self.wall7.center_x -= 1
            self.wall_rise_count += 1
            if self.wall_rise_count == 59:
                self.wall_rise_count = 0
                self.move5_wall = False
                self.move6_wall = True
        if self.move6_wall == True and self.first_time == True:
            self.wall8.center_x += 1
            self.wall_rise_count += 1
            if self.wall_rise_count == 59:
                self.wall_rise_count = 0
                self.move6_wall = False
                self.move7_wall = True
        if self.move7_wall == True and self.first_time == True:
            self.platform2.center_x+=2
            self.wall_rise_count+=1
            if self.wall_rise_count ==30:
                self.wall_rise_count = 0
                self.platform2.center_x-=60
                self.move7_wall = False
                self.move8_wall = True
        if self.move8_wall == True and self.first_time == True:
            self.wall8.center_y -= 5
            self.wall7.center_y -= 5
            self.wall_rise_count += 1
            if self.wall_rise_count == 19:
                self.wall_rise_count = 0
                self.move8_wall = False
                self.move9_wall = True
        if self.move9_wall == True and self.first_time == True:
            self.wall8.center_x -= 5
            self.wall7.center_x += 5
            self.wall_rise_count += 1
            if self.wall_rise_count == 18:
                self.wall_rise_count = 0
                self.move9_wall = False
                self.move10_wall = True
        if self.move10_wall == True and self.first_time == True:
            if self.place == 0:
                self.player_sprite.center_x = 55
                self.player_sprite.center_y = 200
                self.wall1.center_x = 80
                self.wall1.center_y = 300
                self.wall4.center_x = 204
                self.wall4.center_y = 330
                self.wall7.center_x = 450
                self.wall7.center_y = 241
                self.move1 = False
                self.move2 = False
                self.move3 = False
                self.move4 = False
                self.if_place = 0
                self.move5 = False

                self.place+=1
            self.wall9.center_x-=8
            if self.wall9.center_x<720:
                self.wall10.center_x-=8
            if self.wall10.center_x < 720:
                self.wall11.center_x -= 8
            if self.wall11.center_x < 720:
                self.wall12.center_x -= 8
            if self.wall12.center_x < 720:
                self.wall13.center_x -= 8
            if self.wall13.center_x < 720:
                self.wall14.center_x -= 8
            if self.wall14.center_x < 720:
                self.wall15.center_x -= 8
            if self.wall15.center_x < 720:
                self.wall16.center_x -= 8
            if self.wall16.center_x < 720:
                self.wall9.center_x -= 8
            if self.wall9.center_x<-15:
                self.wall9.center_x = 820
            if self.wall10.center_x<-15:
                self.wall10.center_x = 820
            if self.wall11.center_x<-15:
                self.wall11.center_x = 820
            if self.wall12.center_x<-15:
                self.wall12.center_x = 820
            if self.wall13.center_x<-15:
                self.wall13.center_x = 820
            if self.wall14.center_x<-15:
                self.wall14.center_x = 820
            if self.wall15.center_x<-15:
                self.wall15.center_x = 820
            if self.wall16.center_x<-15:
                self.wall16.center_x = 820
        if self.player_sprite.center_x<10 or self.player_sprite.center_y<10 and self.dead == False:
            #self.dead = True
            #self.dead_x = self.player_sprite.center_x
            #self.dead_y = self.player_sprite.center_y
        #if self.death_count == 180:
            if life>1:
                time.sleep(2)
                life-=1
                self.player_sprite.center_x = 55
                self.player_sprite.center_y = 200
                self.place = 0
                self.move1 = False
                self.move2 = False
                self.move3 = False
                self.move4 = False
                self.if_place = 0
                self.move5 = False
                self.first_time = True
                self.wall_rise_count = 0
                self.wall_rise = True
                self.color_trap = (214, 140, 20)
                self.move1_wall = True
                self.move2_wall = False
                self.move3_wall = False
                self.move4_wall = False
                self.move5_wall = False
                self.move6_wall = False
                self.move7_wall = False
                self.move8_wall = False
                self.move9_wall = False
                self.move10_wall = False
                self.death_count = 0
                self.dead = False
                self.setup()
                self.wall1.center_x = 80
                self.wall1.center_y = 300
                self.wall4.center_x = 204
                self.wall4.center_y = 330
                self.wall7.center_x = 450
                self.wall7.center_y = 241
                self.wall8.center_x = 584
                self.wall8.center_y = 241
                self.wall9.center_x = 820
                self.wall10.center_x = 820
                self.wall11.center_x = 820
                self.wall12.center_x = 820
                self.wall13.center_x = 820
                self.wall14.center_x = 820
                self.wall15.center_x = 820
                self.wall16.center_x = 820
                self.setup()
            else:
                time.sleep(2)
                dead = Died()
                self.window.show_view(dead)

        if self.player_sprite.center_x>790:
            time.sleep(2)
            game4 = Game4()
            self.window.show_view(game4)
            game4.setup()

    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        if key == arcade.key.UP or key == arcade.key.W or key == arcade.key.SPACE:
            self.jump = True
    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
            #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-06-30 222800.png")
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False
            #self.player_sprite.texture = arcade.load_texture("屏幕截图 2025-06-30 222800.png")


class Game4(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.left = False
        self.right = False
        self.jump = False
        self.wall_list = None
        self.wall1 = None
        self.player_sprite = None
        self.player_list = None
        self.physical_engine = None
        self.num = 0
        self.platform_list = None
        self.platform1 = None
        self.platform2 = None
        self.platform3 = None
        self.wall2 = None
        self.wall3 = None
        self.wall4 = None
        self.wall5 = None
        self.wall6 = None
        self.wall7 = None
        self.engine = None
        self.go1 = True
        self.wall_move1 = 0
        self.move1 = 0
        self.dead = False
        self.move2 = 0
        self.kill = None
        self.kill_engine = None
        self.go2 = True
        self.go2x = 220
        self.go2y = 470
        self.wall8 = None
        self.wall9 = None
        self.wall10 = None
        self.wall11 = None
        self.wall12 = None
        self.wall13 = None
        self.wall14 = None
        self.wall15 = None
        self.wall16 = None
        self.wall17 = None
        self.wall18 = None
        self.gravity = 0.2
        self.go3 = True
        self.go4 = True
        self.go5 = True
        self.go5x = 660
        self.go5y = 470
        self.go4x = 473
        self.go4y = 420
        self.count = 0
        self.doorl = 135
        self.doorb = 195
        self.less_time = 5
        self.second_count = 0
        self.movetime = 0

    def setup(self):
        self.wall_list = arcade.SpriteList()
        self.player_list = arcade.SpriteList()
        self.platform_list = arcade.SpriteList()
        self.kill = arcade.SpriteList()
        self.player_sprite = arcade.Sprite("屏幕截图 2025-06-12 115306.png")
        self.player_sprite.center_x = 100
        self.player_sprite.center_y = 400
        self.player_list.append(self.player_sprite)
        self.platform1 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.platform1.center_x = 185
        self.platform1.center_y = 380
        self.platform_list.append(self.platform1)
        self.platform2 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.platform2.center_x = 300
        self.platform2.center_y = 380
        self.platform_list.append(self.platform2)
        self.platform3 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.platform3.center_x = 60
        self.platform3.center_y = 380
        self.platform_list.append(self.platform3)
        self.wall6 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.wall6.center_x = 220
        self.wall6.center_y = 450
        self.kill.append(self.wall6)
        self.wall7 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.wall7.center_x = 120
        self.wall7.center_y = 450
        self.kill.append(self.wall7)
        self.wall1 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall1.center_x = 60
        self.wall1.center_y = 440
        self.wall_list.append(self.wall1)
        self.wall2 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall2.center_x = 185
        self.wall2.center_y = 440
        self.wall_list.append(self.wall2)
        self.wall3 = arcade.Sprite("屏幕截图 2025-06-13 160717.png")
        self.wall3.center_x = 386
        self.wall3.center_y = 545
        self.wall_list.append(self.wall3)
        self.wall4 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall4.center_x = 347
        self.wall4.center_y = 410
        self.wall_list.append(self.wall4)
        self.wall5 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall5.center_x = 300
        self.wall5.center_y = 440
        self.wall_list.append(self.wall5)
        self.wall8 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall8.center_x = 60
        self.wall8.center_y = 550
        self.wall_list.append(self.wall8)
        self.wall9 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall9.center_x = 185
        self.wall9.center_y = 550
        self.wall_list.append(self.wall9)
        self.wall10 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.wall10.center_x = 300
        self.wall10.center_y = 550
        self.wall_list.append(self.wall10)
        self.wall11 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall11.center_x =  423
        self.wall11.center_y = 550
        self.wall_list.append(self.wall11)
        self.wall12 = arcade.Sprite("屏幕截图 2025-06-12 182239.png")
        self.wall12.center_x = 630
        self.wall12.center_y = 177
        self.kill.append(self.wall12)
        self.wall13 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall13.center_x = 623
        self.wall13.center_y = 550
        self.wall_list.append(self.wall13)
        self.wall14 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall14.center_x = 753
        self.wall14.center_y = 550
        self.wall_list.append(self.wall14)
        self.wall15 = arcade.Sprite("屏幕截图 2025-06-13 160717.png")
        self.wall15.center_x = 180
        self.wall15.center_y = 183
        self.wall_list.append(self.wall15)
        self.wall16 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall16.center_x = 218
        self.wall16.center_y =180
        self.wall_list.append(self.wall16)
        self.wall17 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.wall17.center_x = 142
        self.wall17.center_y = 180
        self.wall_list.append(self.wall17)
        self.wall18 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.wall18.center_x = 208
        self.wall18.center_y = 180
        self.kill.append(self.wall18)
        self.physical_engine = arcade.PhysicsEnginePlatformer(self.player_sprite, self.platform_list, gravity_constant = 0.2)
        self.engine = arcade.PhysicsEngineSimple(self.player_sprite, self.wall_list)
        self.kill_engine = arcade.PhysicsEngineSimple(self.player_sprite, self.kill)

    def on_draw(self):
        global life
        global times
        global all_time
        self.clear()
        self.kill.draw()
        self.wall_list.draw()
        self.player_list.draw()
        self.platform_list.draw()
        if self.movetime == 0:
            times = 0
        self.movetime+=1
        if self.movetime%60 == 0 and self.movetime!=0:
            times+=1
            all_time+=1
        arcade.draw_lrbt_rectangle_filled(self.doorl, self.doorl+15, self.doorb, self.doorb+30, (255,255,215))
        arcade.draw_text(f"life = {life}", 100, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-4',400,550,arcade.color.BLACK,15,2)
        arcade.draw_text(f'time:{times}', 250, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}', 0, 0, arcade.color.BLACK, 15, 2)
        if self.move1 == 10:
            arcade.draw_text(f"time left = {self.less_time}", 100, 390, arcade.color.BLACK, 15, 2)
        if self.go1 == True:
            arcade.draw_circle_filled(25, 410, 10, (214,140,20))
        if self.go2 == True:
            arcade.draw_circle_filled(self.go2x, self.go2y, 10, (214,140,20))
        if self.go3 == True:
            arcade.draw_circle_filled(340, 582, 10, (214,140,20))
        if self.go4 == True:
            arcade.draw_circle_filled(self.go4x, self.go4y, 10, (214,140,20))
        if self.go5 == True:
            arcade.draw_circle_filled(self.go5x, self.go5y, 10, (214,140,20))
        arcade.draw_circle_filled(790, 420, 10, (214,140,20))

    def on_update(self, delta_time):
        if self.move1 == 5:
            self.count += 1
        if self.move1 == 10:
            self.second_count += 1
        if self.second_count % 60 == 0 and self.move1 == 10:
            self.less_time -= 1
        if self.less_time == 0:
            self.dead = True
        kill_list = arcade.check_for_collision_with_list(self.player_sprite, self.kill)
        global life
        self.physical_engine.update()
        self.engine.update()
        self.kill.update()
        if self.left == True and self.player_sprite.center_x > 8:
            self.player_sprite.center_x -= 5
        if self.right == True and self.player_sprite.center_x < 800:
            self.player_sprite.center_x += 5
        if self.jump == True:
            if self.player_sprite.center_y < 600:
                self.player_sprite.center_y += 8
                self.num += 1
                if self.num == 8:
                    self.num = 0
                    self.jump = False
        if 45 > self.player_sprite.center_x > 25 and self.player_sprite.center_y > 405 and self.player_sprite.center_y < 420:
            self.player_sprite.center_x = 400
            self.player_sprite.center_y = 300
            self.go1 = False
        if 350 > self.player_sprite.center_x > 300 and self.player_sprite.center_y > 405 and self.player_sprite.center_y < 420 and self.move1 == 0:
            self.wall5.center_x -= 5
            self.wall_move1 += 1
            if self.wall_move1 == 10:
                self.wall_move1 = 0
                self.move1 += 1
        if self.player_sprite.center_x > 0 and self.player_sprite.center_y > 420 and self.move1 == 1:
            self.wall5.center_x += 5
            self.wall_move1 += 1
            if self.wall_move1 == 10:
                self.wall_move1 = 0
                self.move1 += 1
        if 200 < self.player_sprite.center_x < 270 and self.player_sprite.center_y > 460 and self.move1 == 2:
            self.wall6.center_y += 10
            self.go2x -= 100
            self.move1 += 1
        if 100 < self.player_sprite.center_x < 150 and self.player_sprite.center_y > 460 and self.move2 == 0:
            self.wall7.center_y += 10
            self.go2x  = 25
            self.move2 += 1
        if self.player_sprite.center_x < 30 and self.player_sprite.center_y > 450 and self.move1 == 3:
            self.player_sprite.center_x = 20
            self.player_sprite.center_y = 585
            self.move1 += 1
            self.go2 = False
        if self.player_sprite.center_x > 100 and self.move1 == 4:
            self.wall10.center_x += 2
            self.wall_move1 += 1
            if self.wall_move1 == 15:
                self.wall_move1 = 0
                self.wall10.center_x -= 30
                self.move1 += 1
        if self.move2 == 1 and self.player_sprite.center_x > 330:
            self.player_sprite.center_x = 400
            self.move2 += 1
            self.go3 = False
        if self.move1 == 6 and self.player_sprite.center_y < 500:
            self.go4x += 40
            self.move1 += 1
        if self.go5x - 20 < self.player_sprite.center_x < self.go5x + 20 and self.go5y - 20 < self.player_sprite.center_y < self.go5y + 20:
            self.player_sprite.center_x = 300
            self.player_sprite.center_y = 200

        if self.count == 30:
            self.wall8.center_x -= 200
            self.wall9.center_x -= 300
            self.wall10.center_x -= 400
            self.count = 0
            self.move1 += 1
        if self.move1 == 7 and self.go4x - 20 < self.player_sprite.center_x < self.go4x + 20 and self.go4y - 20 < self.player_sprite.center_y < self.go4y + 20:
            self.player_sprite.center_x = 623
            self.player_sprite.center_y = 570
            self.move1 += 1
        if self.move1 == 8:
            self.go4x += 0.5
            if self.go4x - 20 < self.player_sprite.center_x < self.go4x + 20 and self.go4y - 20 < self.player_sprite.center_y < self.go4y + 20:
                self.player_sprite.center_x = 753
                self.player_sprite.center_y = 570
                self.move1 += 1
        if self.move1 == 9 and 770 < self.player_sprite.center_x < 810 and 400 < self.player_sprite.center_y < 440:
            self.player_sprite.center_x = 228
            self.player_sprite.center_y = 200
            self.move1 += 1
        if self.move1 == 10:
            if self.player_sprite.center_x > 360:
                self.player_sprite.center_x = 0
            if self.player_sprite.center_y < -10:
                self.player_sprite.center_y = 350

        if self.doorl - 10 < self.player_sprite.center_x < self.doorl + 20 and self.doorb + 9 < self.player_sprite.center_y < self.doorb + 30:
            time.sleep(2)
            choosage1 = Game5()
            self.window.show_view(choosage1)
            choosage1.setup()
        if self.move2 == 2 and self.player_sprite.center_x > 625 and self.player_sprite.center_y < 575:
            self.go5x -= 70
            self.move2 += 1
        for xyz in kill_list:
            if self.move1 < 10:
                self.dead = True
            if self.move1 == 10:
                if xyz == self.wall18:
                    self.dead = True

        if self.move1 < 10:
            if self.player_sprite.center_y < 0:
                self.dead = True

        if self.dead == True:
            if life>1:
                time.sleep(2)
                self.go5 = True
                self.less_time = 5
                self.go5x = 660
                self.go5y = 470
                self.go2 = True
                self.go3 = True
                self.go4 = True
                self.go4x = 473
                self.go4y = 420
                self.wall8.center_x = 60
                self.wall9.center_x = 185
                self.wall10.center_x = 300
                self.dead = False
                self.go2x = 220
                self.player_sprite.center_x = 100
                self.player_sprite.center_y = 410
                self.wall5.center_x = 300
                self.wall6.center_x = 220
                self.wall6.center_y = 450
                self.wall7.center_x = 120
                self.wall7.center_y = 450

                life -= 1
                self.left = False
                self.right = False
                self.jump = False
                self.num = 0
                self.go1 = True
                self.wall_move1 = 0
                self.move1 = 0
                self.dead = False
                self.move2 = 0
            else:
                time.sleep(2)
                dead = Died()
                self.window.show_view(dead)


    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        if key == arcade.key.UP or key == arcade.key.W or key == arcade.key.SPACE:
            self.jump = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False



class Game5(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.left = False
        self.right = False
        self.jump = False
        self.player_list = None
        self.player_sprite = None
        self.platform_list = None
        self.platform1 = None
        self.platform2 = None
        self.platform3 = None
        self.platform4 = None
        self.wall_list = None
        self.wall1 = None
        self.wall2 = None
        self.wall3 = None
        self.num  = 0
        self.physics_engine = None
        self.engine = None
        self.dead = False
        self.move1 = 0
        self.move_count = 0
        self.move_num = 0
        self.move_true = False
        self.jump_true = False
        self.rotate = False
        self.movetime = 0
        self.camera = arcade.camera.Camera2D()


    def setup(self):
        self.player_list = arcade.SpriteList()
        self.platform_list = arcade.SpriteList()
        self.wall_list = arcade.SpriteList()
        self.player_sprite = arcade.Sprite("屏幕截图 2025-06-12 115306.png")
        self.player_sprite.center_x = 400
        self.player_sprite.center_y = 393
        self.player_list.append(self.player_sprite)
        self.platform1 = arcade.Sprite("屏幕截图 2025-06-17 105227.png")
        self.platform1.center_x = 293
        self.platform1.center_y = 198
        self.platform_list.append(self.platform1)
        self.wall1 = arcade.Sprite("屏幕截图 2025-06-17 070320.png")
        self.wall1.center_x = 432
        self.wall1.center_y = 200
        self.wall_list.append(self.wall1)
        self.platform2 = arcade.Sprite("屏幕截图 2025-06-17 105227.png")
        self.platform2.center_x = 571
        self.platform2.center_y = 198
        self.platform_list.append(self.platform2)
        self.platform3 = arcade.Sprite("屏幕截图 2025-06-17 105227.png")
        self.platform3.center_x = 870
        self.platform3.center_y = 198
        self.platform_list.append(self.platform3)
        self.platform4 = arcade.Sprite("屏幕截图 2025-06-17 105227.png")
        self.platform4.center_x = 870
        self.platform4.center_y = 198
        self.platform_list.append(self.platform4)
        self.wall2 = arcade.Sprite("屏幕截图 2025-06-17 070320.png")
        self.wall2.center_x = 870
        self.wall2.center_y = 200
        self.wall_list.append(self.wall2)
        self.physics_engine = arcade.PhysicsEngineSimple(self.player_sprite, self.wall_list)
        self.engine = arcade.PhysicsEnginePlatformer(self.player_sprite,self.platform_list,gravity_constant = 1)


    def on_draw(self):
        global life
        global times
        global all_time
        if self.movetime == 0:
            times = 0
        self.movetime+=1
        if self.movetime%60 == 0 and self.movetime!=0:
            times+=1
            all_time+=1
        self.clear()
        self.camera.use()
        self.platform_list.draw()
        self.wall_list.draw()
        arcade.draw_lrbt_rectangle_filled(1000,1015,385,415,(255,255,215))
        arcade.draw_text(f"life = {life}", self.player_sprite.center_x-250, 600, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-5',self.player_sprite.center_x-50,600,arcade.color.BLACK,15,2)
        arcade.draw_text(f'time:{times}', self.player_sprite.center_x-150, 600, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}', self.player_sprite.center_x-400, 100, arcade.color.BLACK, 15, 2)
        self.player_list.draw()

    def on_update(self, delta_time):
        global life
        global  place
        self.physics_engine.update()
        self.window.set_fullscreen()
        self.camera.viewport = self.window.rect
        self.camera.projection = arcade.LRBT(-400, 400, -300, 300)
        self.engine.update()
        if self.move1 == 3:
            self.move_count+=1
        if self.left == True:
            self.player_sprite.center_x -= 5
        if self.right == True:
            self.player_sprite.center_x += 5
        if self.engine.can_jump() and self.jump_true == False:
            self.jump_true = True

        if self.jump == True and self.jump_true == True:
            if 390<self.player_sprite.center_y <399 or self.num>0:
                self.player_sprite.center_y += 10
                self.num += 1
                if self.num == 8:
                    self.num = 0
                    self.jump = False
        if self.player_sprite.center_y<100:
            self.dead = True
        if self.move1 == 0:
            if self.player_sprite.center_x>410:
                self.wall1.center_x+=200
                self.move1+=1
        if self.move1 ==1:
            if self.player_sprite.center_x>600:
                self.wall1.center_y+=60
                self.move1+=1
        if self.move1 == 2:
            self.wall1.center_x-=10
            self.move_count+=1
            if self.move_count == 17:
                self.move_count = 0
                self.move1+=1
        if self.move_count == 60:
            self.move_count = 0
            self.move1+=1
        if self.move1 == 4:
            self.wall1.center_y-=5
            self.move_count+=1
            if self.move_count == 12:
                self.move_count=0
                self.move1+=1
        if self.player_sprite.center_x>415 and self.move1 == 5:
            self.move_true = True
        if self.move_true == True:
            self.wall1.center_y+=10
            self.move_count+=1
            if self.move_count == 6:
                self.move_count = 0
                self.move1+=1
                self.move_true = False
        if self.move1 == 6:
            self.move_count+=1
        if self.move_count == 30:
            self.move_count = 0
            self.move1+=1
        if self.move1 == 7:
            self.wall1.center_y-=5
            self.move_count+=1
            if self.move_count == 12:
                self.move_count = 0
                self.move1+=1
        if self.move1 == 8:
            self.wall1.center_x-=3
            self.move_count+=1
            if self.move_count == 10:
                self.move_count = 0
                self.move1+=1
        if self.player_sprite.center_x>415 and self.move1 == 9:
            self.wall1.center_x-=100
            self.move1+=1
        if self.move1 == 10:
            self.wall1.center_y+=5
            self.move_count+=1
            if self.move_count == 12:
                self.move_count = 0
                self.move1+=1
        if self.move1 == 11:
            self.wall1.center_x+=3
            self.move_num+=1
            if self.move_num == 80:
                self.move_num = 0
                self.move1+=1
        if self.move1 == 12:
            self.wall1.center_y-=5
            self.wall2.center_y+=10
            self.move_num+=1
            if self.move_num == 6:
                self.move_num = 0
                self.move1+=1
        if self.move1 == 13:
            self.wall1.center_x+=3
            self.wall2.center_x-=3
            self.move_num+=1
            if self.move_num == 36:
                self.move_num = 0
                self.move1+=1
        if self.move1 == 14:
            self.wall2.center_y -= 60
            self.wall1.center_y += 30
            self.move1 += 1
        if self.move1 == 15:
            self.wall1.center_x-=3
            self.move_num+=1
            if self.move_num == 83:
                self.wall1.center_x+=1
                self.move_num = 0
                self.move1+=1
        if self.move1 == 16:
            self.wall1.angle+=6
            self.move_num+=1
            if self.move_num == 30:
                self.move_num = 0
                self.move1+=1

        if self.move1 == 17:
            self.wall1.center_y-=60
            self.move1+=1
        if self.move1 == 18 and self.player_sprite.center_x>550:
            self.platform4.center_x-=5
            self.platform2.center_x-=5
            self.move_num+=1
            if self.move_num == 30:
                self.move_num = 0
                self.move1+=1
        if self.move1 == 19:
            self.platform4.center_x+=3
            self.move_num+=1
            if self.move_num == 40:
                self.move_num = 0
                self.move1+=1
        if self.move1 == 20:
            self.platform4.center_x-=150
            self.platform3.center_x+=140
            self.move1+=1
        if 1030>self.player_sprite.center_x>1010:
            place = 'Difficult Jumping'
            time.sleep(2)

            choosage1 = Choose()
            self.window.show_view(choosage1)
            #choosage1.setup()

        if self.dead == True:
            time.sleep(2)
            if life >1:
                self.setup()
                self.wall1.angle = 0
                self.move_count = 0
                self.move_num = 0
                self.move1 = 0
                self.dead = False
                life-=1
            else:
                time.sleep(2)
                dead = Died()
                self.window.show_view(dead)

        self.camera.position = self.player_sprite.center_x, 400

    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        if key == arcade.key.UP or key == arcade.key.W or key == arcade.key.SPACE:
            self.jump = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False
class Choose(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.camera = arcade.camera.Camera2D()
    def on_draw(self):
        self.clear()
        self.camera.use()
        arcade.draw_lrbt_rectangle_filled(0,800,400,600,(214,140,20))
        arcade.draw_lrbt_rectangle_filled(0,800,0,125,(214,140,20))
        arcade.draw_circle_filled(100,300,10,(214,140,20))
        arcade.draw_circle_filled(200,300,10,(214,140,20))
        arcade.draw_text(f'Your next level is {place}',200,500,arcade.color.BLACK,25,2)
        arcade.draw_text("press c to continue              press q to quit",100,60,arcade.color.BLACK,15,2)
        arcade.draw_text("press 1 for playing first level",500,60,arcade.color.BLACK,15,2)
        arcade.draw_text("Easy Jumping",75,260,arcade.color.BLACK,8,2)
        arcade.draw_text("Difficult Jumping",175,260,arcade.color.BLACK,8,2)
        arcade.draw_line(100,300,200,300,arcade.color.RED,2)
    def on_update(self,delta_time):
        self.camera.position = 400,300
        self.camera.viewport = self.window.rect
        self.camera.projection = arcade.LRBT(-400, 400, -300, 300)


    def on_key_press(self, key, modifiers):
        if key == arcade.key.C:
            self.camera.position = 0,100
            game6 = Life()
            self.window.show_view(game6)
        if key == arcade.key.Q:
            arcade.close_window()
        if key == arcade.key.KEY_1:
            os.execv(sys.executable, ['python'] + sys.argv)
class Game6(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.platform1 = None
        self.platform2 = None
        self.platform3 = None
        self.platform4 = None
        self.platform_list = None
        self.player_sprite = None
        self.player_list = None
        self.engine = None
        self.wall1 = None
        self.wall2 = None
        self.kill_list = None
        self.kill = None
        self.move_count = 0
        self.wall_list = None
        self.physics_engine = None
        self.left = False
        self.right = False
        self.jump = False
        self.jump_true = False
        self.num = 0
        self.number = 0
        self.move1 = 0
        self.count1 = 0
        self.count1_move = False
        self.slow_move_speed = 3
        self.push_true = False
        self.button_list = None
        self.button1 = None
        self.kill1 = None
        self.death = False
        self.jump_height = 6
        self.time = 0
        self.kill2 =None
        self.could_left = 0
        self.door_x = 290
        self.platform5 = None
        self.movetime = 0

    def setup(self):
        self.platform_list = arcade.SpriteList()
        self.wall_list = arcade.SpriteList()
        self.player_list = arcade.SpriteList()
        self.kill_list = arcade.SpriteList()
        self.button_list = arcade.SpriteList()
        self.player_sprite = arcade.Sprite("屏幕截图 2025-06-12 115306.png")
        self.player_sprite.center_x = 70
        self.player_sprite.center_y = 300
        self.player_list.append(self.player_sprite)
        self.platform1 = arcade.Sprite("屏幕截图 2025-06-12 213756.png")
        self.platform1.center_x = 100
        self.platform1.center_y = 100
        self.platform_list.append(self.platform1)
        self.platform2 = arcade.Sprite("屏幕截图 2025-06-17 070320.png")
        self.platform2.center_x = 362
        self.platform2.center_y = 101
        self.platform_list.append(self.platform2)
        self.platform3 = arcade.Sprite("屏幕截图 2025-06-17 105227.png")
        self.platform3.center_x = 533
        self.platform3.center_y = 99
        self.platform_list.append(self.platform3)
        self.wall1 = arcade.Sprite("屏幕截图 2025-06-17 070320.png")
        self.wall1.center_x = 394
        self.wall1.center_y = 101
        self.wall_list.append(self.wall1)
        self.platform4 = arcade.Sprite("屏幕截图 2025-06-17 070320.png")
        self.platform4.center_x = 260
        self.platform4.center_y = 101
        self.platform_list.append(self.platform4)
        self.platform5 = arcade.Sprite("屏幕截图 2025-06-12 213756.png")
        self.platform5.center_x = 680
        self.platform5.center_y = 100
        self.platform_list.append(self.platform5)
        self.button1 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.button1.center_x = 120
        self.button1.center_y = 1000
        self.button_list.append(self.button1)
        for i in range(300,460,32):
            self.kill = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
            self.kill.angle = 270
            self.kill.center_x = 1000
            self.kill.center_y = i
            self.kill_list.append(self.kill)
        self.kill1 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.kill1.center_x = 16
        self.kill1.center_y = 280
        self.kill_list.append(self.kill1)
        self.kill2 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.kill2.center_x = 362
        self.kill2.center_y = 1000
        self.kill_list.append(self.kill2)
        self.physics_engine = arcade.PhysicsEngineSimple(self.player_sprite,self.wall_list)
        self.engine = arcade.PhysicsEnginePlatformer(self.player_sprite,self.platform_list,gravity_constant = 0.1)

    def on_draw(self):
        global life
        global place
        global times
        global all_time
        if self.movetime == 0:
            times = 0
            all_time = 0
        self.movetime+=1
        if self.movetime%60 == 0 and self.movetime != 0:
            times+=1
            all_time+=1

        self.clear()
        self.kill_list.draw()
        self.button_list.draw()
        self.player_list.draw()
        self.platform_list.draw()
        self.wall_list.draw()
        arcade.draw_lrbt_rectangle_filled(750,765,self.door_x,self.door_x+30,(255,255,215))
        arcade.draw_text(f"life = {life}", 100, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-1', 400, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time:{times}', 250, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}', 0, 0, arcade.color.BLACK, 15, 2)
        if self.move1 == 9:
            self.door_x+=0.3


    def on_update(self,delta_time):
        if self.num == 0:
            self.jump_true = False
        global  life
        self.physics_engine.update()
        self.engine.update()
        if self.move1 == 8:
            self.time+=1
        if self.time == 90 and self.move1 == 8:
            self.move1+=1
        if 750<self.player_sprite.center_x<765 and self.door_x<self.player_sprite.center_y<self.door_x+30:
            game7 = Game7()
            self.window.show_view(game7)
            game7.setup()
        self.death_list = arcade.check_for_collision_with_list(self.player_sprite, self.kill_list)
        for zzz in self.death_list:
            self.death = True
        if self.player_sprite.center_y<0 or self.player_sprite.center_y>600 or self.player_sprite.center_x>800:
            self.death = True
        if self.death == True:
            if life>1:
                life-=1
                time.sleep(2)
                self.death = False
                self.setup()
                self.move1 = 0
                self.jump_height = 6
                self.slow_move_speed = 3
                self.move_count = 0
                self.num = 0
                self.number = 0
                self.move1 = 0
                self.count1 = 0
                self.push_true = False
                self.count1_move = False
                self.right = False
                self.could_left = 0
                self.door_x = 290
                self.time = 0
            else:
                dead = Died()
                self.window.show_view(dead)

        #for x in
        #for ad in self.kill_list:
            #ad.center_x-=3
        if self.move1 == 3:
            self.move_count+=1
        if self.move_count == 30:
            self.push_true = True
        if self.move1 == 6:
            self.count1+=1
        if self.count1 == 30:
            self.count1_move = True
        if 8>self.move1>4:
            self.slow_move_speed = 0.5
        if self.move1 == 5:
            self.count1+=1
        if self.count1 == 60:
            self.count1_move = True
        if self.left and self.could_left == 0:
            self.player_sprite.center_x -= self.slow_move_speed
        if self.right:
            self.player_sprite.center_x += self.slow_move_speed
        if self.engine.can_jump() and self.jump_true == False:
            self.jump_true = True
        if self.move1 == 9:
            self.jump = True
            self.jump_true = True
            self.jump_height+=0.005
            self.slow_move_speed = 3
        if self.jump:
            self.player_sprite.center_y += self.jump_height
            self.num += 1
            if self.num == 8:
                self.num = 0
                self.jump = False
                self.jump_true = False
        if self.move1 == 0 and self.player_sprite.center_x>230:
            self.platform2.center_x-=32
            self.move1+=1
        if self.move1 ==1 and self.player_sprite.center_x>320:
            self.platform2.center_x+=32
            self.wall1.center_x+=32
            self.move1+=1
        if self.move1 == 2 and self.player_sprite.center_x>387:
            self.wall1.center_y+=64
            self.move1+=1
        if self.move1 == 3 and self.push_true == True:
            self.platform4.center_y+=5
            self.number+=1
            if self.number == 7:
                self.number = 0
                self.move1+=1
        if self.move1 == 4:
            self.platform4.center_x+=6
            self.number+=1
            if self.number == 17:
                self.number = 0
                self.move1+=1
        if self.move1 == 5 and self.count1_move == True:
            self.platform4.center_y+=2
            self.number+=1
            if self.number == 64:
                self.number = 0
                self.count1 = 0
                self.count1_move = False
                self.move1+=1
        if self.move1 == 6 and self.count1_move == True:
            for abd in self.kill_list:
                if abd != self.kill1:
                    if abd == self.kill2:
                        abd.center_y = 453
                    else:
                        abd.center_x = 342
            self.button1.center_y = 290
            self.move1+=1
        if self.move1 == 7:
            for abb in self.kill_list:
                if abb!=self.kill1:
                    abb.center_x-=1.5
            self.platform4.center_x-=1.5
            if 114<self.player_sprite.center_x<126 and self.player_sprite.center_y<305:
                self.move1+=1
                self.button1.center_y-=10
                self.slow_move_speed = 10
                self.kill1.center_y+=12
    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        if key == arcade.key.UP or key == arcade.key.W or key == arcade.key.SPACE:
            if self.jump_true:
                self.jump = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False
class Game7(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237, 189, 101))
        self.player_sprite = None
        self.player_list = None
        self.wall_list = None
        self.wall1 = None
        self.wall2 = None
        self.wall3 = None
        self.wall4 = None
        self.wall5 = None
        self.wall6 = None
        self.platform_list = None
        self.platform1 = None
        self.platform2 = None
        self.platform3 = None
        self.turn_list = None
        self.engine = None
        self.physics_engine = None
        self.turn1 =  None
        self.left = False
        self.right = False
        self.jump = False
        self.jump_true = False
        self.num = 0
        self.slow_move_speed = 5
        self.jump_height = 6
        self.first_button = True
        self.open = False
        self.move1 = 0
        self.count_death = 0
        self.death = False
        self.wait_time = 0
        self.platform3 = None
        self.platform4 = None
        self.platform5 = None
        self.platform6 = None
        self.platform7 = None
        self.platform8 = None
        self.platform9 = None
        self.platform10 = None
        self.platform11 = None
        self.platform12 = None
        self.platform13 = None
        self.platform14 = None
        self.platform15 = None
        self.player_y = 1000
        self.turn2 = None
        self.camera = None
        self.open2 = False
        self.wait_for_move = 0
        self.move_down = 0
        self.count = 0
        self.door_x = 752.5
        self.door_y = -94
        self.time = 0
        self.shift1 = 0
        self.show = False
        self.movetime = 0
        self.camera_x = 400
        self.shift = 0
        self.high_jump = False
        self.up = 0
        self.first_height = 0
        self.move_back = False
        self.little_move = 0
        self.high_jump_1 = 0
    def setup(self):
        self.player_list = arcade.SpriteList()
        self.platform_list = arcade.SpriteList()
        self.wall_list = arcade.SpriteList()
        self.turn_list = arcade.SpriteList()
        self.player_sprite = arcade.Sprite("屏幕截图 2025-06-12 115306.png")
        self.player_sprite.center_x = 40
        self.player_sprite.center_y = 400
        self.player_list.append(self.player_sprite)
        self.wall5 = arcade.Sprite("屏幕截图 2025-06-13 183516.png")
        self.wall5.center_x = -95
        self.wall5.center_y = 200
        self.wall_list.append(self.wall5)
        self.wall6 = arcade.Sprite("屏幕截图 2025-06-13 183516.png")
        self.wall6.center_x = 895
        self.wall6.center_y = 200
        self.wall_list.append(self.wall6)
        self.wall1 = arcade.Sprite("屏幕截图 2025-06-18 215814.png")
        self.wall1.center_x = 400
        self.wall1.center_y = 530
        self.wall1.scale = 4.0
        self.wall_list.append(self.wall1)
        self.wall2 = arcade.Sprite("屏幕截图 2025-06-12 114644.png",scale = 2.0)
        self.wall2.center_x = 400
        self.wall2.center_y = 450
        self.wall_list.append(self.wall2)
        self.wall3 = arcade.Sprite("屏幕截图 2025-06-29 122040.png")
        self.wall3.center_x = 208
        self.wall3.center_y = 69
        self.wall_list.append(self.wall3)
        self.wall4 = arcade.Sprite("屏幕截图 2025-06-29 122040.png")
        self.wall4.center_x = 592
        self.wall4.center_y = 69
        self.wall_list.append(self.wall4)
        self.platform15 = arcade.Sprite("屏幕截图 2025-06-29 214556.png")
        self.platform15.center_x = 264
        self.platform15.center_y = 15
        self.wall_list.append(self.platform15)
        self.platform14 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.platform14.center_x = 700
        self.platform14.center_y = 140
        self.wall_list.append(self.platform14)
        self.platform10 = arcade.Sprite("屏幕截图 2025-06-12 114736.png",scale = 2.0)
        self.platform10.center_x = 300
        self.platform10.center_y = -26
        self.platform_list.append(self.platform10)
        self.platform11 = arcade.Sprite("屏幕截图 2025-06-12 114736.png",scale = 2.0)
        self.platform11.center_x = 503
        self.platform11.center_y = -26
        self.platform_list.append(self.platform11)
        self.platform12 = arcade.Sprite("屏幕截图 2025-06-12 114736.png",scale = 2.0)
        self.platform12.center_x = 400
        self.platform12.center_y = 100
        self.platform_list.append(self.platform12)
        self.platform9 = arcade.Sprite("屏幕截图 2025-06-12 114736.png",scale = 2.0)
        self.platform9.center_x = 750
        self.platform9.center_y = 100
        self.platform_list.append(self.platform9)
        self.platform8 = arcade.Sprite("屏幕截图 2025-06-12 114736.png")
        self.platform8.scale = 2.0
        self.platform8.center_x = 50
        self.platform8.center_y = 100
        self.platform_list.append(self.platform8)
        self.platform7 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.platform7.center_x = 550
        self.platform7.center_y = 350
        self.platform7.angle = 180
        self.platform_list.append(self.platform7)
        self.platform6 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.platform6.center_x = 200
        self.platform6.center_y = 250
        self.platform_list.append(self.platform6)
        self.platform5 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.platform5.center_x = 300
        self.platform5.center_y = 350
        self.platform5.angle = 180
        self.platform_list.append(self.platform5)
        self.platform4 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.platform4.center_x = 400
        self.platform4.center_y = 250
        self.platform_list.append(self.platform4)
        self.platform1 = arcade.Sprite("屏幕截图 2025-06-18 215814.png")
        self.platform1.scale = 1.5
        self.platform1.center_x = 400
        self.platform1.center_y = 235
        self.platform_list.append(self.platform1)
        self.platform2 = arcade.Sprite("屏幕截图 2025-06-19 171125.png")
        self.platform2.center_x = 400
        self.platform2.center_y = 390
        self.platform_list.append(self.platform2)
        self.platform3 = arcade.Sprite("屏幕截图 2025-06-18 215814.png")
        self.platform3.scale = 1.5
        self.platform3.center_x = 400
        self.platform3.center_y = 360
        self.platform_list.append(self.platform3)
        self.turn1 = arcade.Sprite("0ac2e9556fa316635a1bfb1e943585c.png",scale = 0.5)
        self.turn1.center_x = 460
        self.turn1.center_y = 360
        self.turn_list.append(self.turn1)
        self.turn2 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.turn2.center_x = 550
        self.turn2.center_y = 350
        self.turn2.angle = 180
        self.turn_list.append(self.turn2)
        self.platform13 = arcade.Sprite("屏幕截图 2025-06-18 215814.png")
        self.platform13.scale = 1.5
        self.platform13.center_x = 400
        self.platform13.center_y = 110
        self.platform_list.append(self.platform13)
        self.engine = arcade.PhysicsEnginePlatformer(self.player_sprite,self.platform_list,gravity_constant = 0.1)
        self.physics_engine = arcade.PhysicsEngineSimple(self.player_sprite,self.wall_list)
        self.camera = arcade.camera.Camera2D()
    def on_draw(self):
        global life
        global place
        global times
        global all_time
        if self.movetime == 0:
            times = 0
        self.movetime+=1
        if self.movetime%60 == 0 and self.movetime != 0:
            times+=1
            all_time+=1
        self.turn1.angle+=20
        self.clear()
        self.camera.use()
        self.turn_list.draw()
        self.wall_list.draw()
        self.platform_list.draw()
        self.player_list.draw()
        arcade.draw_lrbt_rectangle_filled(self.door_x,self.door_x+15,self.door_y,self.door_y+30,(255,255,215))
        arcade.draw_text(f"life = {life}", 100,self.player_y + 150, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-2', 400, self.player_y + 150, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time:{times}', 250,self.player_y + 150, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}',0, self.player_y-400, arcade.color.BLACK,
                         15, 2)

        if self.show == True:
            arcade.draw_circle_filled(740,-55,10,(214,140,20))
    def on_update(self,delta_time):
        global life
        self.physics_engine.update()
        self.engine.update()
        self.camera.viewport = self.window.rect
        self.camera.projection = arcade.LRBT(-500, 500, -300, 300)
        if self.player_sprite.collides_with_sprite(self.platform15):
            self.platform15.texture = arcade.load_texture("屏幕截图 2025-07-01 201701.png")
            self.platform15.center_y+=5
            self.high_jump_1 = 0
            self.jump_true = True
            self.jump = True
            self.high_jump = True
        if self.player_sprite.collides_with_sprite(self.platform14):
            self.platform14.center_y = 127
            self.move_down=1
        if self.move_down == 1:
            if self.platform13.center_y>-120:
                self.platform13.center_y-=5
        if self.player_sprite.center_y<self.player_y and self.player_y>200:
            self.player_y = self.player_sprite.center_y
        '''if self.move1>19:
            if self.shift == 0:
                self.camera_x += 10
            elif self.shift == 1:
                self.camera_x-=10
            if self.shift1 == 0:
                self.player_y+=10
            elif self.shift1 == 1:
                self.player_y-=10
            if self.shift == 0:
                self.shift = 1
            else:
                self.shift = 0
            if self.shift1 == 0:
                self.shift1 = 1
            else:
                self.shift1 = 0 '''
        if self.player_y<255:
            self.player_y = 255
        self.camera.position = self.camera_x,self.player_y-100
        if self.player_sprite.collides_with_sprite(self.turn2):
            self.death = True
        if self.death == True:
            if life>1:
                self.setup()
                time.sleep(2)
                self.move1 = 0
                self.open = False
                self.left = False
                self.right = False
                self.jump = False
                self.jump_true = False
                self.num = 0
                self.slow_move_speed = 5
                self.jump_height = 6
                self.first_button = True
                self.death = False
                self.wait_time = 0
                self.count_death = 0
                self.player_y = 1000
                self.open2 = False
                self.move_down = 0
                self.wait_for_move = 0
                self.count = 0
                self.door_x = 752.5
                self.door_y = -94
                life-=1
                self.shift = 0
                self.shift1 = 0
                self.camera_x = 400
                self.move_back = False
                self.little_move = 0
                self.high_jump_1 = 0
                self.show = False
            else:
                time.sleep(2)
                dead = Died()
                self.window.show_view(dead)
        if self.player_sprite.collides_with_sprite(self.turn1):
            self.death = True
        if self.left and self.player_sprite.center_x>7.5:
            self.player_sprite.center_x -= self.slow_move_speed
        if self.right and self.player_sprite.center_x<792.5:
            self.player_sprite.center_x += self.slow_move_speed
        if self.engine.can_jump() and self.jump_true == False:
            self.jump_true = True
        if self.platform2.center_y == 382 and self.move1 == 0:
            self.open = True
            self.turn1.center_y+=10
            self.slow_move_speed = 1
        if self.turn1.center_y>750:
            self.move1+=1
        if self.jump:
            if self.high_jump == False:
                self.player_sprite.center_y += self.jump_height
                self.num += 1
                if self.num == 8:
                    self.num = 0
                    self.jump = False
                    self.jump_true = False
            else:
                if not self.engine.can_jump():
                    self.player_sprite.center_y += 7.5
                    self.high_jump_1+=1
                    if self.high_jump_1 == 8:
                        self.platform15.texture = arcade.load_texture("屏幕截图 2025-06-29 214556.png")
                        self.platform15.center_y  = 15

                #self.high_jump_1+=1
                if self.engine.can_jump():
                    self.high_jump_1 = 0
                    self.jump = False
                    self.high_jump = False
                    self.jump_true = False
                #self.up = self.player_sprite.center_y

        if self.player_sprite.collides_with_sprite(self.platform2) == True and self.first_button == True:
            self.first_button = False
            self.platform2.center_y-=8
            self.slow_move_speed = 5
        if self.move1 == 1:
            self.turn1.center_x = -50
            self.turn1.center_y = 420
            self.turn1.scale = 1.5
            self.move1+=1
        elif self.move1 == 2:
            self.turn1.center_x+=2
            if self.player_sprite.center_x>730:
                self.move1+=1
        elif self.move1 == 3:
            self.platform3.center_x-=250
            self.move1+=1
        elif self.move1 == 4:
            if self.turn1.center_x<750:
                self.turn1.center_x+=2
            else:
                self.move1+=1
        elif self.move1 == 5:
            self.turn1.scale=1
            self.turn1.center_y-=2
            if self.turn1.center_y < 235:
                self.turn1.scale = 0.3
                self.slow_move_speed = 5
                self.move1+=1
        elif self.move1 == 6:
            self.turn1.center_y = 265
            self.turn1.center_x = 550
            self.move1+=1
        elif self.move1 == 7:
            self.turn1.center_x+=5
            if self.turn1.center_x > 830:
                self.move1+=1
        elif self.move1 == 8:
            self.platform4.center_y+=15
            self.move1+=1
        elif self.move1 == 9:
            if self.right == True:
                self.death = True
            if self.player_sprite.collides_with_sprite(self.platform4):
                self.platform4.center_y-=8
                self.platform5.center_y-=20
                self.move1+=1
        elif self.move1 == 10:
            if self.right:
                self.death = True
            if self.player_sprite.collides_with_sprite(self.platform5):
                self.platform5.center_y+=8
                self.platform6.center_y+=15
                self.move1+=1
        elif self.move1 == 11:
            if self.right:
                self.death = True
            if self.player_sprite.collides_with_sprite(self.platform6):
                self.platform6.center_y-=8
                self.platform7.center_y-=20
                self.move1+=1
        elif self.move1 == 12:
            if self.left:
                self.count_death+=1
                if self.count_death == 5:
                    self.death = True
            if 560>self.player_sprite.center_x>540 and self.player_sprite.center_y>280:
                self.platform7.center_x+=100
                self.turn2.center_y-=21
                self.move1+=1
        elif self.move1 == 13:
            if self.player_sprite.collides_with_sprite(self.platform7):
                self.platform7.center_y+=8
                self.turn2.center_x-=350
                self.turn2.center_y = 250
                self.move1+=1
        elif self.move1 == 14:
            self.turn1.scale = 1.5
            self.turn1.center_x-=5
            if self.player_sprite.center_x<225:
                self.platform6.center_y-=10
                self.turn2.center_y+=15
                self.turn2.angle = 0
                self.move1+=1
        elif self.move1 == 15:
            if self.turn1.center_x>50:
                self.turn1.center_x-=5
                if self.player_sprite.center_x<70 and self.open2 == False:
                    self.platform1.center_x+=250
                    self.open2 = True
            else:
                self.move1+=1
        elif self.move1 == 16:
            if self.wait_for_move<30:
                self.wait_for_move+=1
            else:
                if self.turn1.center_y>120:
                    self.turn1.center_y-=2
                else:
                    self.move1+=1
        elif self.move1 == 17:
            if self.turn1.center_x<400:
                self.turn1.center_x+=5
            else:
                self.move1+=1
        elif self.move1 == 18 and self.move_down == 1:
            if self.little_move == 0:
                self.slow_move_speed = 2.5
            if self.player_sprite.center_y<140 and self.little_move == 0:
                if 450>self.player_sprite.center_x>350:
                    self.turn1.scale = 2.0
            elif self.player_sprite.center_x<500 and self.little_move == 0:
                self.turn1.center_y = 130
                self.turn1.scale = 0.3
                self.slow_move_speed = 5
                self.little_move+=1
            if self.little_move == 1:
                if self.player_sprite.center_x<220:
                    self.move_back = True
                if self.move_back == True and self.turn1.center_x>0:
                    self.turn1.center_x-=10
                elif self.turn1.center_x<=0:
                    self.little_move+=1
            if self.little_move == 2:
                self.turn1.scale = 2.0
                if self.turn1.center_x<600:
                    self.turn1.center_x+=2
                else:
                    self.turn1.center_x = 400
                    self.turn1.center_y = 130
                    self.turn1.scale = 0.3
                    self.little_move+=1
            if self.little_move == 3:
                if 380<self.player_sprite.center_x<420 and self.player_sprite.center_y>140:
                    self.turn1.scale = 1.5
                if self.player_sprite.center_x<150:
                    self.move1+=1


        if self.move1 == 19:
            if self.player_sprite.center_x<100:
                self.platform8.center_x+=125
                self.turn1.center_y = -110
                self.turn1.center_x = 600
                self.move1+=1
        elif self.move1 == 20 and self.player_sprite.center_y<50:
            if self.player_sprite.center_x>400:
                self.turn1.center_y+=35
                self.move1+=1
        elif self.move1 == 21:
            if self.turn1.center_x>100:
                self.turn1.center_x-=5
            else:
                self.move1+=1
        elif self.move1 == 22:
            self.turn2.center_x = 16
            self.turn2.center_y = -90
            self.count+=1
            if self.count == 60:
                self.turn1.center_x = 740
                self.turn1.center_y = -110
                self.move1+=1
        elif self.move1 == 23:
            if self.player_sprite.center_x>700:
                self.turn1.center_y+=15
                self.move1+=1
        elif self.move1 == 24 and self.player_sprite.center_x>750:
            if self.show:
                time.sleep(1)
                self.player_sprite.center_x = 100
                self.player_sprite.center_y = -80
                self.turn1.center_x = -80
                self.turn1.center_y = -60
                self.turn1.scale = 1.5
                self.move1+=1
            self.show = True
        elif self.move1 == 25:
            self.show = False
            if self.player_sprite.center_x>200:
                self.turn1.center_x+=6
                if self.player_sprite.center_x>740:
                    self.door_x+=22.5
                    self.turn2.center_x = 755
                    self.turn2.center_y = -90
                    self.move1+=1
        elif self.move1 == 26:
            if self.player_sprite.center_x>785:
                time.sleep(2)
                game8 = Game8()
                self.window.show_view(game8)
                game8.setup()



    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        if key == arcade.key.UP or key == arcade.key.W or key == arcade.key.SPACE:
            if self.jump_true:
                self.jump = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False
class Game8(arcade.View):
    def __init__(self):
        super().__init__()
        arcade.set_background_color((237,189,101))
        self.player_sprite = None
        self.player_list = None
        self.wall_list = None
        self.wall1 = None
        self.wall2 = None
        self.platform_list = None
        self.platform1 = None
        self.platform2 = None
        self.platform3 = None
        self.platform4 = None
        self.platform5 = None
        self.platform6 = None
        self.platform7 = None
        self.platform8 = None
        self.platform9 = None
        self.physics_engine = None
        self.engine = None
        self.camera_go = False
        self.left = False
        self.right = False
        self.jump = False
        self.jump_true = False
        self.num = 0
        self.move1 = 0
        self.up_true = False
        self.count = 0
        self.camera_x = 0
        self.wait1 = 0
        self.wait2 = 0
        self.wait3 = 0
        self.platforma1 = None
        self.pymunk_engine = None
        self.moveing_platform = arcade.SpriteList()
        self.camera = arcade.camera.Camera2D()
        self.door_x = 3352
        self.door_y = 357.5
        self.go1 = False
        self.wall3 = None
        self.dead = False
        self.movetime = 0
    def setup(self):
        self.pymunk_engine = arcade.PymunkPhysicsEngine(gravity=(0,-1000))
        self.player_list = arcade.SpriteList()
        self.platform_list = arcade.SpriteList()
        self.wall_list = arcade.SpriteList()
        self.player_sprite = arcade.Sprite("屏幕截图 2025-06-12 115306.png")
        self.player_sprite.center_x = 100
        self.player_sprite.center_y = 200
        self.player_list.append(self.player_sprite)
        self.pymunk_engine.add_sprite(self.player_sprite,body_type=arcade.PymunkPhysicsEngine.DYNAMIC,friction=0.2,mass= 10,elasticity=0)
        self.wall1 = arcade.Sprite("屏幕截图 2025-06-13 183109.png")
        self.wall1.center_x = -100
        self.wall1.center_y = 300
        self.platform_list.append(self.wall1)
        self.platform1 = arcade.Sprite("屏幕截图 2025-06-12 182239.png")
        self.platform1.center_x = 0
        self.platform1.center_y = 50
        self.platform_list.append(self.platform1)
        self.platform2 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.platform2.center_x = 100
        self.platform2.center_y = 200
        self.platform_list.append(self.platform2)
        self.platform3 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.platform3.center_x = 128
        self.platform3.center_y = 200
        self.platform_list.append(self.platform3)
        self.platform4 = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
        self.platform4.center_x = 156
        self.platform4.center_y = 200
        self.platform_list.append(self.platform4)
        self.platform5 = arcade.Sprite("屏幕截图 2025-06-12 213756(1).png")
        self.platform5.center_x = 1521
        self.platform5.center_y = 253
        self.platform_list.append(self.platform5)
        self.platform6 = arcade.Sprite("屏幕截图 2025-06-12 213756(1).png")
        self.platform6.center_x = 1549
        self.platform6.center_y = 253
        self.platform_list.append(self.platform6)
        self.platform7 = arcade.Sprite("屏幕截图 2025-06-12 213756(1).png")
        self.platform7.center_x = 1577
        self.platform7.center_y = 253
        self.platform_list.append(self.platform7)
        self.platform8 = arcade.Sprite("屏幕截图 2025-06-12 213756(1).png")
        self.platform8.center_x = 1633
        self.platform8.center_y = 253
        self.platform_list.append(self.platform8)
        self.platforma1 = arcade.Sprite("屏幕截图 2025-06-12 182239.png")
        self.platforma1.center_x = 3000
        self.platforma1.center_y = 169.75
        self.platform_list.append(self.platforma1)
        self.pymunk_engine.add_sprite(self.platforma1,body_type=arcade.PymunkPhysicsEngine.STATIC)
        #1121
        #1521
        self.wall2 = arcade.Sprite("屏幕截图 2025-06-12 213756(1).png",scale=2.0)
        self.wall2.center_x = 1703
        self.wall2.center_y = 365
        self.wall_list.append(self.wall2)
        self.wall3 = arcade.Sprite("屏幕截图 2025-06-14 170204.png")
        self.wall3.center_x = 0
        self.wall3.center_y = 0
        self.wall_list.append(self.wall3)

        for i in range(20):
            self.wall = arcade.Sprite("屏幕截图 2025-07-02 125551.png")
            self.wall.center_x = i*700
            self.wall.center_y = 650
            self.wall_list.append(self.wall)
        self.physics_engine = arcade.PhysicsEnginePlatformer(self.player_sprite,self.platform_list,gravity_constant=0.5,walls = self.wall_list)
        for a in range(100):
            self.walll = arcade.Sprite("屏幕截图 2025-06-12 114644.png")
            self.walll.center_x = a*100
            self.walll.center_y = 513
            self.wall_list.append(self.walll)
            self.moveing_platform.append(self.walll)
        self.abcdefg = []
        for ife in range(500):
            n = ife+3000
            self.abcdefg.append(n)
    def on_draw(self):
        global life
        global times
        global all_time
        if self.movetime == 0:
            times = 0
        self.movetime+=1
        if self.movetime%60 == 0 and self.movetime!=0:
            times+=1
            all_time+=1
        self.clear()
        self.camera.use()
        self.player_list.draw()
        self.platform_list.draw()
        self.wall_list.draw()
        arcade.draw_text(f"life = {life}", self.player_sprite.center_x-150, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'Level:{place}-3',self.player_sprite.center_x,550,arcade.color.BLACK,15,2)
        arcade.draw_text(f'time:{times}', self.player_sprite.center_x+250, 550, arcade.color.BLACK, 15, 2)
        arcade.draw_text(f'time in all level:{all_time}', self.player_sprite.center_x-300, 0, arcade.color.BLACK, 15, 2)
        if self.move1>14:
            arcade.draw_lrbt_rectangle_filled(self.door_x,self.door_x+15,self.door_y,self.door_y+30,(255,255,215))
    def on_update(self, delta_time):
        if self.player_sprite.center_y<0:
            self.dead = True
        if self.dead:
            time.sleep(2)
            if life>1:
                self.setup()
                self.camera_go = False
                self.left = False
                self.right = False
                self.jump = False
                self.jump_true = False
                self.num = 0
                self.move1 = 0
                self.up_true = False
                self.count = 0
                self.camera_x = 0
                self.wait1 = 0
                self.wait2 = 0
                self.wait3 = 0
                self.door_x = 3352
                self.door_y = 357.5
                self.go1 = False
                self.dead = False
            else:
                dead = Died()
                self.window.show_view(dead)
        self.physics_engine.update()


        self.camera_x = self.player_sprite.center_x
        self.camera.position = self.camera_x+100,300
        self.camera.viewport = self.window.rect
        self.camera.projection = arcade.LRBT(-400, 400, -300, 300)
        if self.left:
            self.player_sprite.center_x -= 3
        if self.right:
            self.player_sprite.center_x +=3
        if self.physics_engine.can_jump() and self.jump_true == False:
            self.jump_true = True
        if self.jump:
            if self.jump_true == True:
                self.physics_engine.jump(5)
                self.num+=1
                if self.num == 8:
                    self.num = 0
                    self.jump = False
                    self.jump_true = False
        if self.move1 == 0:
            if self.player_sprite.center_x>=200:
                self.up_true = True
            if self.up_true == True:

                self.platform2.center_y+=3
                self.platform3.center_y+=3
                self.platform4.center_y+=3
                self.count+=1
                if self.count == 17:
                    self.count = 0
                    self.move1+=1
        elif self.move1 == 1:
            self.platform2.center_x += 5
            self.platform3.center_x += 5
            self.platform4.center_x += 5
            self.count+=1
            if self.count == 20:
                self.platform2.center_x -= 1
                self.platform3.center_x -= 1
                self.platform4.center_x -= 1
                self.count = 0
                self.move1+=1
        elif self.move1 == 2:
            if self.wait1<30:
                self.wait1+=1
            if self.wait1 == 30:
                self.platform2.center_y += 5
                self.platform3.center_y += 5
                self.platform4.center_y += 5
                self.count+=1
                if self.count == 25:
                    self.count = 0
                    self.move1+=1
        elif self.move1 == 3:
            if 403>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=2
            self.platform2.center_x += 2
            self.platform3.center_x += 2
            self.platform4.center_x += 2
            if self.player_sprite.center_x>800:
                self.move1+=1
        elif self.move1 == 4:
            if 403>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=2
            if 250>self.player_sprite.center_y>self.platform2.center_y+25:
                self.player_sprite.center_x+=2
            self.platform2.center_x += 2
            self.platform3.center_x += 2
            self.platform4.center_x += 2
            self.platform2.center_y-=5
            self.count+=1
            if self.count == 50:
                self.count = 0
                self.move1+=1
        elif self.move1 == 5:
            if 403>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=2
            if 250>self.player_sprite.center_y>self.platform2.center_y+25:
                self.player_sprite.center_x+=2
            self.platform2.center_x += 2
            self.platform3.center_x += 2
            self.platform4.center_x += 2
            if self.wait2<60:
                self.wait2+=1
            if self.wait2 == 60:
                self.platform4.center_y -= 5
                self.count += 1
                if self.count == 50:
                    self.platform2.center_x+=14
                    self.platform4.center_x-=14
                    self.count = 0
                    self.move1 += 1
        elif self.move1 == 6:
            if 403>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=2
            if 180>self.player_sprite.center_y>self.platform2.center_y+25:
                self.player_sprite.center_x+=2
            self.platform2.center_x += 2
            self.platform3.center_x += 2
            self.platform4.center_x += 2
            if self.platform2.center_x >= 1549:
                self.platform2.center_y-=30
                self.platform4.center_y-=30
                self.move1+=1
        elif self.move1 == 7:
            if 403>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=2
            self.platform3.center_x+=2
            if self.platform3.center_x == 1633:
                self.platform3.center_y = 250
                self.move1+=1
        elif self.move1 == 8:
            if self.wait3<30:
                self.wait3+=1
            if self.wait3 == 30:
                self.platform2.center_x+=2
                self.platform4.center_x+=2
                if self.player_sprite.center_x>1633 and self.player_sprite.center_y<320:
                    self.platform3.center_x+=4
                    self.count+=1
                    if self.count==7:
                        self.count = 0
                        self.move1+=1
                if self.player_sprite.center_x<1633 and self.player_sprite.center_y<320:
                    self.move1+=1
        elif self.move1 == 9:
            if 180>self.player_sprite.center_y>self.platform2.center_y+25:
                self.player_sprite.center_x+=2
            self.platform2.center_x+=2
            self.platform4.center_x+=2
            if self.player_sprite.center_x>2100:
                self.move1+=1
        elif self.move1 == 10:
            self.number =(self.walll.center_y-self.player_sprite.center_y)/20
            for abcde in self.moveing_platform:
                abcde.center_y = 122.5
            self.move1+=1
        elif self.move1 == 11:
            self.platform3.center_x = self.platform2.center_x-28
            self.platform3.center_y = self.platform2.center_y
            if 123>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=2
            self.platform2.center_x += 2
            self.platform3.center_x += 2
            self.platform4.center_x += 2
            if self.player_sprite.center_x>2500:
                self.move1+=1
        elif self.move1 == 12:
            self.platform2.center_y += 5
            self.platform3.center_y += 5
            self.platform4.center_y += 5
            self.count+=1
            if self.count == 50:
                self.count = 0
                self.move1+=1
        elif self.move1 == 13:
            if 403>self.player_sprite.center_y>self.platform3.center_y+25:
                self.player_sprite.center_x+=10
            self.platform2.center_x+=10
            self.platform3.center_x+=10
            self.platform4.center_x+=10
            self.count+=1
            if self.player_sprite.center_x>2800:
                self.count = 0
                self.move1+=1
        elif self.move1 == 14:
            if self.player_sprite.center_x>3100:
                self.platform2.center_y = 343.5
                self.platform2.center_x = 3283
                self.platform3.center_y = 343.5
                self.platform3.center_x = 3311
                self.platform4.center_y = 343.5
                self.platform4.center_x = 3339
                self.move1+=1
        elif self.move1 == 15:
            if self.player_sprite.center_x>3342:
                self.door_x-=112
                self.platform3.center_x-=28
                self.move1+=1
        elif self.move1 == 16:
            if self.player_sprite.center_x<3315 and self.go1 == False:
                self.wall3.center_x = 3283
                self.wall3.center_y = 355
                self.wall3.center_y+=7
                self.platform3.center_y+=7
                self.go1 = True
            if self.go1==True:
                self.wall3.center_y+=7
                self.platform3.center_y+=7
                if self.platform3.center_y>550:
                    self.move1+=1
        elif self.move1 == 17:
            self.platform3.center_x+=5
            if self.platform3.center_x>4000:
                self.move1+=1
        if self.player_sprite.collides_with_sprite(self.wall3):
            self.dead = True
        elif self.move1 == 18:
            if self.door_x+15>self.player_sprite.center_x>self.door_x and self.door_y+30>self.player_sprite.center_y>self.door_y:
                time.sleep(2)
                game9 = Game9()
                self.window.show_view(game9)
                game9.setup()
    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = True
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = True
        if key == arcade.key.UP or key == arcade.key.W or key == arcade.key.SPACE:
            #if self.jump_true:
            self.jump = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT or key == arcade.key.A:
            self.left = False
        elif key == arcade.key.RIGHT or key == arcade.key.D:
            self.right = False

def main():
    window = arcade.Window(800,600, "Game",fullscreen = False,resizable = True)
    start_view = Game8()
    window.show_view(start_view)
    start_view.setup()
    arcade.run()

if __name__ == "__main__":
    main()

