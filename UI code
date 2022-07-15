-- Start Table
local library = {};
-- debug service
local getupvalues		= getupvalues or debug.getupvalues;
local getupvalue 		= getupvalue or debug.getupvalue;
local setupvalue 		= setupvalue or debug.setupvalue;
local setconstant		= setconstant or debug.setconstant;
local getconstant		= getconstant or debug.getconstant;
local getconstants		= getconstants or debug.getconstants;
local getgc 			= getgc;
local getinfo			= getinfo or debug.getinfo;
local getreg			= getreg or debug.getregistry;
local getproto			= getproto or debug.getproto;
local setproto			= setproto or debug.setproto;
local is_exploit_caller = checkcaller;
local is_synapse_function = (is_synapse_function or issentinelclosure);
local game				= game;
local next 				= next;
local newcclosure 		= newcclosure;
local pcall 			= pcall;
local rawget 			= rawget;
local require 			= require;
local select 			= select;
local typeof 			= typeof;
local hookfunction		= hookfunction;
local getfenv 			= getfenv;
local getsenv			= getsenv;
local hookmetamethod 	= hookmetamethod or function(tbl, mt, func) return hookfunction(getrawmetatable(tbl)[mt], func) end;
local getrenv			= getrenv;

-- Services
local TweenService      = game:GetService("TweenService");
local UserInputService  = game:GetService("UserInputService");
local Mouse             = game:GetService("Players").LocalPlayer:GetMouse();
local CoreGui           = game:GetService("CoreGui");
local HttpService       = game:GetService("HttpService");
local RunService        = game:GetService("RunService");

-- Tween Func
local function Tween(obj, time, tab)
    if (not obj or not time or not tab) then return; end;

    TweenService:Create(obj, TweenInfo.new(time), tab):Play();
end;

-- Instance Creation
function library:Create(class, prop)
    local object    = Instance.new(class);
    prop            = typeof(prop) == "table" and prop or {};

    if (class == "ScreenGui" and syn) then
        syn.protect_gui(object);
    end;

    for i, v in next, prop do
        object[i] = v;
    end;

    return object;
end;

-- Main UI Build
do
    -- Screen GUI Creation
    local MainScreenGUI = library:Create("ScreenGui", {
        Name            = HttpService:GenerateGUID(true);
        Parent          = CoreGui;
        DisplayOrder    = 1;
        ResetOnSpawn    = false;
    });

    -- Main Frame
    local MainFrame = library:Create("Frame", {
        Name                = "Main";
        Parent              = MainScreenGUI;
        AnchorPoint         = Vector2.new(0.5, 0.5);
        BackgroundColor3    = Color3.new(0.117647, 0.117647, 0.117647);
        BorderColor3        = Color3.new(0.211765, 0.211765, 0.211765);
        Position            = UDim2.new(0.5, 0, 0.5, 0);
        Size                = UDim2.new(0, 434, 0, 263);
        Active              = true;
        Draggable           = true;
    });
    
    
    local closebutton = Instance.new("TextButton")
    closebutton.Name = "CloseButton"
    closebutton.Parent = MainFrame
    closebutton.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
    closebutton.BorderSizePixel = 0
    closebutton.Position = UDim2.new(0.955, 0, 0, 0)
    closebutton.Size = UDim2.new(0, 20, 0, 20)
    closebutton.Font = Enum.Font.SourceSansSemibold
    closebutton.Text = "X"
    closebutton.TextColor3 = Color3.fromRGB(255, 255, 255)
    closebutton.TextScaled = true
    closebutton.TextStrokeTransparency = 0
    closebutton.TextWrapped = true


closebutton.MouseButton1Down:connect(function()
MainFrame.Visible = false
end)
    

    -- Tab Frame
    local TabsFrame = library:Create("Frame", {
        Name                        = "Tabs";
        Parent                      = MainFrame;
        BackgroundColor3            = Color3.new(1, 1, 1);
        BackgroundTransparency      = 1;
        Size                        = UDim2.new(0, 463, 0, 48);
    });

    -- UIPadding Creation
    library:Create("UIPadding", {
        Parent          = TabsFrame;
        PaddingLeft     = UDim.new(0, 10);
    });

    -- UIListLayout Creation
    library:Create("UIListLayout", {
        Parent              = TabsFrame;
        FillDirection       = Enum.FillDirection.Horizontal;
        SortOrder           = Enum.SortOrder.LayoutOrder;
        VerticalAlignment   = Enum.VerticalAlignment.Center;
    });

    -- Tab Holder Frame
    local TabHolderFrame = library:Create("Frame", {
        Name                = "Tabholders";
        Visible             = true;
        Parent              = MainFrame;
        BackgroundColor3    = Color3.new(0.101961, 0.101961, 0.101961);
        BorderColor3        = Color3.new(0.156863, 0.156863, 0.156863);
        Position            = UDim2.new(0.0223880615, 0, 0.182509512, 0);
        Size                = UDim2.new(1, -18, 0, 205);
    });

    -- UI Lib Functions
    do
        -- AddTab Func
        function library:AddTab(name)
            local TextButtonEz = library:Create("TextButton", {
                Name                        = ("Tab" .. name);
                Parent                      = TabsFrame;
                BackgroundTransparency      = 1;
                BackgroundColor3            = Color3.new(0.091, 0.091, 0.091);
                BorderColor3                = Color3.new(0.137255, 0.137255, 0.137255);
                Size                        = UDim2.new(0, 124, 0, 28);
                AutoButtonColor             = false;
                Font                        = Enum.Font.SourceSansSemibold;
                Text                        = name;
                TextColor3                  = Color3.new(0.921569, 0.921569, 0.921569);
                TextSize                    = 17;
                TextStrokeColor3            = Color3.new(0.203922, 0.203922, 0.203922);
                TextStrokeTransparency      = 0.64999997615814;
            });

            -- Containers
            local Container = library:Create("ScrollingFrame", {
                Name                    = ("Tab" .. name);
                Parent                  = TabHolderFrame;
                Active                  = true;
                Visible                 = false;
                BackgroundColor3        = Color3.new(1, 1, 1);
                BackgroundTransparency  = 1;
                BorderSizePixel         = 0;
                Size                    = UDim2.new(1, 0, 1, 0);
                ScrollBarThickness      = 0;
                Active                  = true;
            });

            -- UIPadding Creation
            library:Create("UIPadding", {
                Parent          = Container;
                PaddingTop      = UDim.new(0, 8);
            });

            -- UIListLayout Creation
            library:Create("UIListLayout", {
                Parent              = Container;
                FillDirection       = Enum.FillDirection.Vertical;
                SortOrder           = Enum.SortOrder.LayoutOrder;
                VerticalAlignment   = Enum.VerticalAlignment.Top;
                HorizontalAlignment = Enum.HorizontalAlignment.Center;
                Padding             = UDim.new(0, 5);
            });

            -- Loads First Tab
            for i, v in next, TabsFrame:GetChildren() do
                if (v:IsA("TextButton")) then
                    Tween(v, 0.2, {
                        BackgroundTransparency = 0;
                    });
                    break;
                end;
            end;

            for i, v in next, TabHolderFrame:GetChildren() do
                v.Visible = true;
                break;
            end;

            -- Connection
            TextButtonEz.MouseButton1Click:Connect(function()
                for i, v in next, TabHolderFrame:GetChildren() do
                    v.Visible = false;
                end;

                for i, v in next, TabsFrame:GetChildren() do
                    if (v:IsA("TextButton")) then
                        Tween(v, 0.2, {
                            BackgroundTransparency = 1;
                        });
                    end;
                end;

                task.wait(0.2);

                Tween(TextButtonEz, 0.2, {BackgroundTransparency = 0});
                Container.Visible = true;
            end);

            -- UI Funcs
            local T2 = {};

            -- Add Toggle Func
            function T2:AddToggle(options)
                options = options or {
                    Text = "";
                    Callback = function()
                    end;
                    Enabled = false;
                };

                local MainToggleFrame = library:Create("Frame", {
                    Name                = ("Toggle" .. options.Text);
                    Parent              = Container;
                    BackgroundColor3    = Color3.new(0.092, 0.092, 0.092);
                    BorderColor3        = Color3.new(0.12549, 0.12549, 0.12549);
                    Size                = UDim2.new(1, -14, 0, 32);
                });

                local TextLabelStuff = library:Create("TextLabel", {
                    Name                    = ("Toggle" .. options.Text);
                    Parent                  = MainToggleFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1.0199999809265;
                    Position                = UDim2.new(0, 10, 0, 0);
                    Size                    = UDim2.new(0, 200, 0, 32);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Text;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Left;
                });

                -- TextButton
                local TextButtonThingy = library:Create("TextButton", {
                    Parent                  = MainToggleFrame;
                    BackgroundColor3        = Color3.new(0.093, 0.093, 0.093);
                    BackgroundTransparency  = 1;
                    Size                    = UDim2.new(1, 0, 1, 0);
                    Font                    = Enum.Font.SourceSans;
                    TextColor3              = Color3.new(0, 0, 0);
                    TextSize                = 14;
                    TextTransparency        = 1;
                });

                -- Switch Frame
                local MainSwitchFrame = library:Create("Frame", {
                    Name                = "Switchbackground";
                    Parent              = MainToggleFrame;
                    BackgroundColor3    = Color3.new(0.12549, 0.12549, 0.12549);
                    BorderSizePixel     = 0;
                    Position            = UDim2.new(0.878000021, -8, 0.25, 0);
                    Size                = UDim2.new(0, 41, 0, 16);
                });

                local MinSwitchFrame = library:Create("Frame", {
                    Name                = "Switch";
                    Parent              = MainSwitchFrame;
                    BackgroundColor3    = options.Enabled and Color3.fromRGB(235, 235, 235) or Color3.new(0.0941176, 0.0941176, 0.0941176);
                    BorderSizePixel     = 0;
                    Position            = UDim2.new(0, 2, 0.125, 0);
                    Size                = UDim2.new(0, 18, 0, 12);
                });

                -- Connection
                local state = options.Enabled or false;

                TextButtonThingy.MouseButton1Click:Connect(function()
                    state = not state;

                    if (state) then
                        Tween(MinSwitchFrame, 0.2, {
                            Position            = UDim2.new(0.439, 2, 0.125, 0);
                            BackgroundColor3    = Color3.fromRGB(235, 235, 235);
                        });
                    else
                        Tween(MinSwitchFrame, 0.2, {
                            Position            = UDim2.new(0, 2, 0.125, 0);
                            BackgroundColor3    = Color3.new(0.0941176, 0.0941176, 0.0941176);
                        });
                    end;

                    options.Callback(state);
                end);
            end;

            -- Textbox Func
            function T2:AddTextbox(options)
                options = options or {
                    Text        = "";
                    Callback    = function() end;
                    StartText   = "";
                };

                local MainFrameText = library:Create("Frame", {
                    Name                = ("Textframe" .. options.Text);
                    Parent              = Container;
                    BackgroundColor3    = Color3.new(0.094, 0.094, 0.094);
                    BorderColor3        = Color3.new(0.12549, 0.12549, 0.12549);
                    Size                = UDim2.new(1, -14, 0, 32);
                });

                -- TextLabel
                library:Create("TextLabel", {
                    Name                    = ("Textbox" .. options.Text);
                    Parent                  = MainFrameText;
                    BackgroundColor3        = Color3.new(0.095, 0.095, 0.095);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0, 10, 0, 0);
                    Size                    = UDim2.new(0, 200, 0, 32);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Text;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Left;
                });

                local BackgroundFrame = library:Create("Frame", {
                    Name                = "Inputbackground";
                    Parent              = MainFrameText;
                    BackgroundColor3    = Color3.new(0.12549, 0.12549, 0.12549);
                    BorderSizePixel     = 0;
                    Position            = UDim2.new(0.785032392, -8, 0.25, 0);
                    Size                = UDim2.new(0, 80, 0, 16);
                    ZIndex              = 2;
                });

                local MainTextbox = library:Create("TextBox", {
                    Name                    = ("Input" .. options.Text);
                    Parent                  = BackgroundFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Size                    = UDim2.new(0, 80, 0, 16);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = (options.StartText and options.StartText or "");
                    Position                = UDim2.new(0, 0, 0, 0);
                    TextColor3              = Color3.new(0.862745, 0.862745, 0.862745);
                    TextSize                = 14;
                    ZIndex                  = 3;
                });

                MainTextbox.FocusLost:Connect(function()
                    pcall(options.Callback, MainTextbox.Text);
                end);
            end;

            -- KeyBind Func
            function T2:AddKeybind(options)
                options = options or {
                    Text        = "";
                    Callback    = function() end;
                    Start       = "F";
                };

                local MainFrame = library:Create("Frame", {
                    Parent              = Container;
                    BackgroundColor3    = Color3.new(0.096, 0.096, 0.096);
                    BorderColor3        = Color3.new(0.12549, 0.12549, 0.12549);
                    Size                = UDim2.new(1, -14, 0, 32);
                });

                library:Create("TextLabel", {
                    Name                    = (options.Text .. "Keybindname");
                    Parent                  = MainFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0, 10, 0, 0);
                    Size                    = UDim2.new(0, 200, 0, 32);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Text;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Left;
                });

                local FrameBackground = library:Create("Frame", {
                    Name                = "Keybackground";
                    Parent              = MainFrame;
                    BackgroundColor3    = Color3.new(0.12549, 0.12549, 0.12549);
                    BorderSizePixel     = 0;
                    Position            = UDim2.new(0.785032392, -8, 0.25, 0);
                    Size                = UDim2.new(0, 80, 0, 16);
                    ZIndex              = 2;
                });

                local TextLabelNeeded = library:Create("TextLabel", {
                    Name                    = "Key";
                    Parent                  = FrameBackground;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Size                    = UDim2.new(1, 0, 1, 0);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Start and options.Start or "...";
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    ZIndex                  = 3;
                });

                local MainTextbutton = library:Create("TextButton", {
                    Parent                  = MainFrame;
                    BackgroundColor3        = Color3.new(0.097, 0.097, 0.097);
                    BackgroundTransparency  = 1;
                    Size                    = UDim2.new(1, 0, 1, 0);
                    Font                    = Enum.Font.SourceSans;
                    TextColor3              = Color3.new(0, 0, 0);
                    TextSize                = 14;
                    TextTransparency        = 1;
                });

                -- Main Stuff
                local con;
                local w = false;

                local function disconnect()
                    if (con) then
                        con:Disconnect();
                        con = nil;
                    end;
                end;

                MainTextbutton.MouseButton1Click:Connect(function()
                    w = true;

                    disconnect();
                    TextLabelNeeded.Text = "...";
                    task.wait();

                    con = UserInputService.InputBegan:Connect(function(key)
                        local keyc = key.KeyCode == Enum.KeyCode.Unknown and key.UserInputType or key.KeyCode;

                        if (keyc.Name ~= "Focus" and keyc.Name ~= "MouseMovement" and keyc.Name ~= "Focus") then
            
                            disconnect();
                            task.wait();
                            TextLabelNeeded.Text = keyc.Name;
                            task.wait();
                            w = false;
                            
                            options.Callback(keyc);
                        end;
                    end);
                end);
            end;

            -- Dropdown Func
            function T2:AddDropdown(options)
                options = options or {
                    Text        = "";
                    Callback    = function() end;
                    Options     = {
                        "1";
                        "2";
                    };
                };

                local DropdownMainFrame = library:Create("Frame", {
                    Name                = (options.Text .. "Dropdown");
                    Parent              = Container;
                    BackgroundColor3    = Color3.new(0.092, 0.092, 0.092);
                    BorderColor3        = Color3.new(0.12549, 0.12549, 0.12549);
                    Size                = UDim2.new(1, -14, 0, 32);
                });

                library:Create("TextLabel", {
                    Name                    = "Dropdownname";
                    Parent                  = DropdownMainFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0, 10, 0, 0);
                    Size                    = UDim2.new(0, 200, 0, 32);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Text;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Left;
                });

                library:Create("ImageLabel", {
                    Parent                  = DropdownMainFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0.953675687, -8, 0.28125, 0);
                    Rotation                = 90;
                    Size                    = UDim2.new(0, 13, 0, 13);
                    Image                   = "rbxassetid://71659683";
                    ImageColor3             = Color3.new(0.921569, 0.921569, 0.921569);
                    ScaleType               = Enum.ScaleType.Fit;
                });

                local DropdownList = library:Create("Frame", {
                    Parent                  = DropdownMainFrame;
                    BackgroundColor3        = Color3.fromRGB(34, 34, 34);
                    BorderSizePixel         = 0;
                    Position                = UDim2.new(0, -1, 1.1, 0);
                    Size                    = UDim2.new(0, 403, 0, 0);
                    ClipsDescendants        = true;
                    ZIndex                  = 3;
                });

                local MainTextbutton = library:Create("TextButton", {
                    Parent                  = DropdownMainFrame;
                    BackgroundColor3        = Color3.new(0.099, 0.099, 0.099);
                    BackgroundTransparency  = 1;
                    Size                    = UDim2.new(1, 0, 1, 0);
                    Font                    = Enum.Font.SourceSans;
                    TextColor3              = Color3.new(0, 0, 0);
                    TextSize                = 14;
                    TextTransparency        = 1;
                });

                -- UIList Creation
                library:Create("UIListLayout", {
                    Parent                  = DropdownList;
                    HorizontalAlignment     = Enum.HorizontalAlignment.Center;
                    SortOrder               = Enum.SortOrder.LayoutOrder;
                });

                -- Main Things
                local BodyYSize     = 5;
                local Opened        = false;
        
                for i, v in next, options.Options do
                    local Button = library:Create("TextButton", {
                        Parent                  = DropdownList;
                        BackgroundColor3        = Color3.new(0.0910, 0.0910, 0.0910);
                        Size                    = UDim2.new(0, 417, 0, 25);
                        Font                    = Enum.Font.SourceSansSemibold;
                        TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                        TextSize                = 15;
                        AutoButtonColor         = false;
                        Text                    = v;
                        BackgroundTransparency  = 1;
                        BorderSizePixel         = 0;
                        ZIndex                  = 3;
                    });
        
                    Button.MouseEnter:Connect(function()
                        Tween(Button, 0.2, {
                            BackgroundTransparency = 0.5;
                        });
                    end);
        
                    Button.MouseLeave:Connect(function()
                        Tween(Button, 0.2, {
                            BackgroundTransparency  = 1;
                            BackgroundColor3        = Color3.new(0.0941176, 0.0941176, 0.0941176);
                        });
                    end);
        
                    Button.MouseButton1Click:Connect(function()
                        Opened = false;

                        Tween(DropdownList, 0.2, {
                            Size = UDim2.new(0, 403, 0, 0);
                        });

                        options.Callback(v);
                    end);
        
                    BodyYSize = BodyYSize + 25;
                end;
        
                MainTextbutton.MouseButton1Click:Connect(function()
                    for i, v in next, DropdownMainFrame:GetChildren() do
                        if (v == DropdownList and v.Size == UDim2.new(0, 403, 0, BodyYSize)) then
                            Tween(v, 0.2, {
                                Size = UDim2.new(0, 403, 0, 0);
                            });
                        end;
                    end;
        
                    Opened = not Opened;

                    if (Opened) then
                        Tween(DropdownList, 0.2, {
                            Size = UDim2.new(0, 403, 0, BodyYSize)
                        });
                    else
                        Tween(DropdownList, 0.2, {
                            Size = UDim2.new(0, 403, 0, 0);
                        });
                    end;
                end);
            end;

            -- Slider Func
            function T2:AddSlider(options)
                options = options or {
                    Text        = "";
                    Min         = 0;
                    Max         = 0;
                    Def         = 0;
                    Callback    = function() end;
                };

                local SliderMainFrame = library:Create("Frame", {
                    Name                = (options.Text .. "Slider");
                    Parent              = Container;
                    BackgroundColor3    = Color3.new(0.0911, 0.0911, 0.0911);
                    BorderColor3        = Color3.new(0.12549, 0.12549, 0.12549);
                    Size                = UDim2.new(1, -14, 0, 50);
                });

                library:Create("TextLabel", {
                    Name                    = "Slidername";
                    Parent                  = SliderMainFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0, 10, 0, 0);
                    Size                    = UDim2.new(0, 200, 0, 32);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Text;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Left;
                });

                local SlidervalueText = library:Create("TextLabel", {
                    Name                    = "Slidervalue";
                    Parent                  = SliderMainFrame;
                    BackgroundColor3        = Color3.new(1, 1, 1);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0, 370, 0, -1);
                    Size                    = UDim2.new(0, 200, 0, 32);
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Def;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Left;
                });

                local SliderBackground = library:Create("Frame", {
                    Name                = "Sliderlinebackground";
                    Parent              = SliderMainFrame;
                    BackgroundColor3    = Color3.new(0.12549, 0.12549, 0.12549);
                    BorderSizePixel     = 0;
                    Position            = UDim2.new(0.0350000188, 0, 0.920000017, -10);
                    Size                = UDim2.new(0, 374, 0, 4);
                });

                local MainSliderP = library:Create("Frame", {
                    Name                = "Sliderlinecolored";
                    Parent              = SliderBackground;
                    BackgroundColor3    = Color3.new(0.784314, 0.784314, 0.784314);
                    BorderSizePixel     = 0;
                    Size                = UDim2.new(((options.Def or options.Min) - options.Min) / (options.Max - options.Min), 0, 0, 4);
                });

                local MainTextbutton = library:Create("TextButton", {
                    Parent                  = SliderMainFrame;
                    BackgroundColor3        = Color3.new(0.0912, 0.0912, 0.0912);
                    BackgroundTransparency  = 1;
                    Position                = UDim2.new(0.0350000188, 0, 0.920000017, -10);
                    Size                    = UDim2.new(0, 374, 0, 4);
                    Font                    = Enum.Font.SourceSans;
                    TextColor3              = Color3.new(0, 0, 0);
                    TextSize                = 14;
                    TextTransparency        = 1;
                });

                -- MainStuff ( aids but idc )
                local Value;
        
                MainTextbutton.MouseButton1Down:Connect(function()
                    down                = true;
                    Value               = math.floor((((tonumber(options.Max) - tonumber(options.Min)) / 375) * MainSliderP.AbsoluteSize.X) + tonumber(options.Min)) or 0;
                    SlidervalueText.Text= tostring(Value);

                    pcall(options.Callback, Value);
                    MainSliderP:TweenSize(UDim2.new(0, math.clamp(Mouse.X - MainSliderP.AbsolutePosition.X, 0, 375), 0, 4), Enum.EasingDirection.InOut, Enum.EasingStyle.Linear, 0.07);

                    while task.wait() and down do
                        Value               = math.floor((((tonumber(options.Max) - tonumber(options.Min)) / 375) * MainSliderP.AbsoluteSize.X) + tonumber(options.Min)) or 0;
                        SlidervalueText.Text= tostring(Value);

                        pcall(options.Callback, Value);
                        MainSliderP:TweenSize(UDim2.new(0, math.clamp(Mouse.X - MainSliderP.AbsolutePosition.X, 0, 375), 0, 4), Enum.EasingDirection.InOut, Enum.EasingStyle.Linear, 0.07);
                    end;
                end);
        
                UserInputService.InputEnded:connect(function(key)
                    if (key.UserInputType == Enum.UserInputType.MouseButton1 and down) then
                        down                = false;
                        Value               = math.floor((((tonumber(options.Max) - tonumber(options.Min)) / 375) * MainSliderP.AbsoluteSize.X) + tonumber(options.Min)) or 0;
                        SlidervalueText.Text= tostring(Value);

                        pcall(options.Callback, Value);
                        MainSliderP:TweenSize(UDim2.new(0, math.clamp(Mouse.X - MainSliderP.AbsolutePosition.X, 0, 375), 0, 4), Enum.EasingDirection.InOut, Enum.EasingStyle.Linear, 0.07);
                    end;
                end);
            end;

            function T2:AddButton(options)
                options = options or {
                    Text        = "";
                    Callback    = function() end;
                };

                -- TextButton
                local TextButtonThingy = library:Create("TextButton", {
                    Parent                  = Container;
                    BackgroundColor3        = Color3.new(0.0913, 0.0913, 0.0913);
                    BorderColor3            = Color3.new(0.12549, 0.12549, 0.12549);
                    BackgroundTransparency  = 0;
                    Size                    = UDim2.new(1, -14, 0, 32);
                    TextTransparency        = 0;
                    Font                    = Enum.Font.SourceSansSemibold;
                    Text                    = options.Text;
                    TextColor3              = Color3.new(0.921569, 0.921569, 0.921569);
                    TextSize                = 15;
                    TextXAlignment          = Enum.TextXAlignment.Center;
                    AutoButtonColor         = true;
                });

                -- Connection
                TextButtonThingy.MouseButton1Click:Connect(function()
                    options.Callback();
                end);
            end;

            return T2;
        end;
    end;
end;

-- examples

--[[local tab = library:AddTab("silver");

tab:AddToggle({
    Text        = "test";
    Callback    = function(state)
        print(state);
    end;
    Enabled     = false;
});

tab:AddTextbox({
    Text        = "hi";
    Callback    = function(text)
        print(text);
    end;
    StartText   = "ez";
});

tab:AddKeybind({
    Text        = "test";
    Callback    = function(ez)
        print(ez);
    end;
    Start       = "F";
});

tab:AddDropdown({
    Text = "d";
    Callback = function(text)
        print(text);
    end;
    Options = {
        "hi";
        "hi1";
        "hi1";
    };
});

tab:AddSlider({
    Text    = "d";
    Min     = 0;
    Max     = 10;
    Def     = 1;
    Callback = function(value)
        print(value);
    end;
});

tab:AddButton({
    Text    = "test";
    Callback = function()
        print("hi");
    end;
});]]

return library;
