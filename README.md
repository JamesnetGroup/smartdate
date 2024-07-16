## Content

- Tutorial Introduction
- Overview
- WPF SmartDate
- Contributor Introduction
- Communication and Support
- Tutorial Series
- Ideas & Feedback

<br/>

![2c06e4f6-8973-4466-84f5-2a6ea45bc3dc](https://github.com/vickyqu115/smartdate/assets/52397976/b848c8ef-b8ad-4925-bae0-f8ab142bf69a)

## Tutorial Introduction
The WPF SmartDate tutorial is provided through videos and a GitHub repository with source code, and we strive to deliver detailed and extensive learning content and updates through Code Project, community channels, Facebook, and blogs.

- [YouTube](https://www.youtube.com/watch?v=oTtM5NNVCNc): English audio, Korean subtitles
- [BiliBili](https://www.bilibili.com/video/BV1pE421L7c2): Chinese audio
- [GitHub](https://github.com/vickyqu115/smartdate): Source code, includes README.md
- [Code-Project](https://www.codeproject.com/search.aspx?q=vickyqu115&x=0&y=0&sbo=kw): Project review pending.

## Overview

This video marks the fifth installment of the WPF tutorial series, starting with ThemeSwitch. We've mainly covered content derived from ContentControl and ItemsControl such as Buttons, Sliders, and ListBoxes. However, this time we delve into more complex functionalities like those in a DatePicker. Therefore, this series will be more challenging than previous ones.

But there's no need to worry. This tutorial video is prepared with care to ensure you can learn through repetition, with all processes, contents, and explanations arranged in an orderly fashion. Just repeat until you become familiar and proficient.

## WPF SmartDate

For nearly 20 years since its inception, most controls derived from Template-including Control, ContentControl, and ItemsControl in WPF have been widely used without significant lack in classes, interfaces, and provided Dependency Property attributes. This shows how precisely WPF is designed. Especially, WPF's Template and hierarchy-focused design structure suit most controls like Buttons, ComboBoxes, and ListBoxes well.

However, controls like DatePicker, which demand complex functionalities and detailed customization, have made us feel the limitations of the basic controls provided. In contrast to [Riot Slider](https://www.youtube.com/watch?v=jyv2fP9TUtY) from a previous tutorial, which featured a monotonous function and simple Template structure making it meaningful to analyze and customize the Template structure, DatePicker operates almost like a small application with various internal processes, making it a very challenging task to extract and analyze the basic Template structure. This will serve as an excellent training in WPF research and analysis.

If you are looking to analyze and study the basic control setup of DatePicker, this is a great approach. Additionally, we recommend examining the SmartDate control method featured in this tutorial. The main processes and content of this video are as follows:

| Order | Main Task | Summary of Main Content |
|:--:|:---|:---|
| 1 | WPF Application | Creation of program entry structure and creation and execution of Application instance Run() |
| 2 | WPF Class Library | Basic structure of CustomControl: AssemblyInfo.cs, Generic.xaml, CustomControl |
| 3 | SmartDate | CustomControl derived from replacing the DatePicker |
| 4 | CalendarSwitch | CustomControl derived from a ToggleButton that manages the popup switching of the SmartDate control |
| 5 | Popup | Basic control encompassing the calendar |
| 6 | CalendarBox | CustomControl derived from a ListBox displaying dates on the calendar, specifying ItemsPresenter's container as a UniformGrid via ItemsPanelTemplate, including moving buttons for the calendar in the ControlTemplate |
| 7 | CalendarBoxItem | CustomControl derived from ListBoxItem which acts as the ItemsContainer for the CalendarBox |
| 8 | ChevronButton | CustomControl derived from a Button that moves to the previous or next month |
| 9 | DayOfWeek | CustomControl derived from a Label that displays the days of the week |

A look at the main tasks shows that not only SmartDate but also its subordinate controls utilize CustomControl. This design provides a good example of the philosophy behind WPF's CustomControl design.

Through this tutorial video, we hope you gain a detailed understanding of the implementation of CustomControl. If you need more preliminary study on the concept of Template, we recommend watching the RiotSlider tutorial video first.

## SmartDate.xaml (Control)
_SmartDateControl/Themes/Units/SmartDate.xaml_
```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:james="https://jamesnet.dev/xaml/presentation"
    xmlns:units="clr-namespace:SmartDateControl.UI.Units">

    <Style TargetType="{x:Type TextBlock}" x:Key="MonthStyle">
        <Setter Property="Text" Value="{Binding RelativeSource={RelativeSource TemplatedParent}, Path=CurrentMonth,StringFormat={}{0:yyyy.MM}}"/>
        <Setter Property="Foreground" Value="#FFFFFF"/>
        <Setter Property="FontSize" Value="13"/>
        <Setter Property="Margin" Value="10 10 10 5"/>
        <Setter Property="VerticalAlignment" Value="Center"/>
        <Setter Property="HorizontalAlignment" Value="Center"/>
    </Style>
    
    <Style TargetType="{x:Type units:SmartDate}">
        <Setter Property="Background" Value="#151515"/>
        <Setter Property="Height" Value="30"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type units:SmartDate}">
                    <Border Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}" CornerRadius="4">
                        <Grid>
                            <units:CalendarSwitch x:Name="PART_Switch"/>
                            <Popup x:Name="PART_Popup" StaysOpen="False" VerticalOffset="2" AllowsTransparency="True">
                                <Border Background="{TemplateBinding Background}" CornerRadius="4" Padding="10">
                                    <james:JamesGrid Rows="Auto,Auto,Auto" Columns="*">
                                        <james:JamesGrid Rows="*" Columns="Auto,*,AUto">
                                            <units:ChevronButton x:Name="PART_Left" Tag="Left"/>
                                            <TextBlock Style="{StaticResource MonthStyle}"/>
                                            <units:ChevronButton x:Name="PART_Right" Tag="Right"/>
                                        </james:JamesGrid>
                                        <UniformGrid Columns="7">
                                            <units:DayOfWeek Grid.Column="0" Content="Su"/>
                                            <units:DayOfWeek Grid.Column="1" Content="Mo"/>
                                            <units:DayOfWeek Grid.Column="2" Content="Tu"/>
                                            <units:DayOfWeek Grid.Column="3" Content="We"/>
                                            <units:DayOfWeek Grid.Column="4" Content="Th"/>
                                            <units:DayOfWeek Grid.Column="5" Content="Fr"/>
                                            <units:DayOfWeek Grid.Column="6" Content="Sa"/>
                                        </UniformGrid>
                                        <units:CalendarBox x:Name="PART_ListBox"/>
                                    </james:JamesGrid>
                                </Border>
                            </Popup>
                        </Grid>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

## SmartDate.cs
_SmartDateControl.UI.Units.SmartDate_
```csharp
﻿using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Controls.Primitives;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace SmartDateControl.UI.Units
{
    public class SmartDate : Control
    {
        private Popup _popup;
        private CalendarSwitch _switch;
        private CalendarBox _listbox;

        public bool KeepPopupOpen
        {
            get { return (bool)GetValue(KeepPopupOpenProperty); }
            set { SetValue(KeepPopupOpenProperty, value); }
        }

        public static readonly DependencyProperty KeepPopupOpenProperty =
            DependencyProperty.Register("KeepPopupOpen", typeof(bool), typeof(SmartDate), new PropertyMetadata(true));



        public DateTime CurrentMonth
        {
            get { return (DateTime)GetValue(CurrentMonthProperty); }
            set { SetValue(CurrentMonthProperty, value); }
        }

        public static readonly DependencyProperty CurrentMonthProperty =
            DependencyProperty.Register("CurrentMonth", typeof(DateTime), typeof(SmartDate), new PropertyMetadata(null));



        public DateTime? SelectedDate
        {
            get { return (DateTime?)GetValue(SelectedDateProperty); }
            set { SetValue(SelectedDateProperty, value); }
        }

        public static readonly DependencyProperty SelectedDateProperty =
            DependencyProperty.Register("SelectedDate", typeof(DateTime?), typeof(SmartDate), new PropertyMetadata(null));



        static SmartDate()
        {
            DefaultStyleKeyProperty.OverrideMetadata(typeof(SmartDate), new FrameworkPropertyMetadata(typeof(SmartDate)));
        }
        public override void OnApplyTemplate()
        {
            base.OnApplyTemplate();

            _popup = (Popup)GetTemplateChild("PART_Popup");
            _switch = (CalendarSwitch)GetTemplateChild("PART_Switch");
            _listbox = (CalendarBox)GetTemplateChild("PART_ListBox");
            ChevronButton leftButton = (ChevronButton)GetTemplateChild("PART_Left");
            ChevronButton rightButton = (ChevronButton)GetTemplateChild("PART_Right");

            _popup.Closed += _popup_Closed;
            _switch.Click += _switch_Click;
            _listbox.MouseLeftButtonUp += _listbox_MouseLeftButtonUp;

            leftButton.Click += (s, e) => MoveMonthClick(-1);
            rightButton.Click += (s, e) => MoveMonthClick(1);

        }

        private void MoveMonthClick(int month)
        {
            GenerateCalendar(CurrentMonth.AddMonths(month));
        }

        private void _popup_Closed(object sender, EventArgs e)
        {
            _switch.IsChecked = IsMouseOver;
        }

        private void _switch_Click(object sender, RoutedEventArgs e)
        {
            if (_switch.IsChecked == true)
            {
                _popup.IsOpen = true;

                GenerateCalendar(SelectedDate ?? DateTime.Now);
            }
        }

        private void _listbox_MouseLeftButtonUp(object sender, MouseButtonEventArgs e)
        {
            if (_listbox.SelectedItem is CalendarBoxItem selected)
            {
                SelectedDate = selected.Date;
                GenerateCalendar(selected.Date);

                _popup.IsOpen = KeepPopupOpen; 
            }
        }
        private void GenerateCalendar(DateTime current)
        {
            if (current.ToString("yyyyMM") == CurrentMonth.ToString("yyyyMM")) return;

            CurrentMonth = current;
            _listbox.Items.Clear();
            DateTime fDayOfMonth = new(current.Year,current.Month,1);
            DateTime lDayOfMonth = fDayOfMonth.AddMonths(1).AddDays(-1);

            int fOffset = (int)fDayOfMonth.DayOfWeek;
            int lOffset = 6 - (int)lDayOfMonth.DayOfWeek;

            DateTime fDay = fDayOfMonth.AddDays(-fOffset);
            DateTime lDay = lDayOfMonth.AddDays(lOffset);

            for (DateTime day = fDay; day <= lDay; day = day.AddDays(1))
            {
                CalendarBoxItem boxItem = new();
                boxItem.Date = day;
                boxItem.DateFormat = day.ToString("yyyyMMdd");
                boxItem.Content = day.Day;
                boxItem.IsCurrentMonth = day.Month == current.Month;

                _listbox.Items.Add(boxItem);
            }
            if (SelectedDate != null)
            {
                _listbox.SelectedValue = SelectedDate.Value.ToString("yyyyMMdd");
            }
        }
    }
}
```

## CalendarSwitch.xaml (ToggleButton)
_SmartDateControl/Themes/Units/CalendarSwitch.xaml_
```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:units="clr-namespace:SmartDateControl.UI.Units">

    <Geometry x:Key="CalendarGeometry">M9,10V12H7V10H9M13,10V12H11V10H13M17,10V12H15V10H17M19,3A2,2 0 0,1 21,5V19A2,2 0 0,1 19,21H5C3.89,21 3,20.1 3,19V5A2,2 0 0,1 5,3H6V1H8V3H16V1H18V3H19M19,19V8H5V19H19M9,14V16H7V14H9M13,14V16H11V14H13M17,14V16H15V14H17Z</Geometry>
    <Geometry x:Key="CalendarSelectionGeometry">M19,19H5V8H19M16,1V3H8V1H6V3H5C3.89,3 3,3.89 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V5C21,3.89 20.1,3 19,3H18V1M17,12H12V17H17V12Z</Geometry>

    <Style TargetType="{x:Type Path}" x:Key="PathStyle">
        <Setter Property="Data" Value="{StaticResource CalendarGeometry}"/>
        <Setter Property="Fill" Value="#AAFFFFFF"/>
        <Setter Property="Width" Value="16"/>
        <Setter Property="Height" Value="16"/>
        <Setter Property="Stretch" Value="Uniform"/>
        <Setter Property="Margin" Value="0 0 6 0"/>
    </Style>

    <Style TargetType="{x:Type TextBlock}" x:Key="SelectedStyle">
        <Setter Property="Text" Value="{Binding RelativeSource={RelativeSource AncestorType=units:SmartDate},Path=SelectedDate, StringFormat={}{0:yyyy-MM-dd}}"/>
        <Setter Property="Foreground" Value="#AAFFFFFF"/>
        <Setter Property="VerticalAlignment" Value="Center"/>
        <Setter Property="Margin" Value="6 0 0 0"/>
    </Style>
    
    <Style TargetType="{x:Type units:CalendarSwitch}">
        <Setter Property="Background" Value="Transparent"/>
        <Setter Property="Foreground" Value="#FFFFFF"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type units:CalendarSwitch}">
                    <Border Background="{TemplateBinding Background}"
                            BorderBrush="{TemplateBinding BorderBrush}"
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <Grid>
                            <Grid.ColumnDefinitions>
                                <ColumnDefinition Width="*"/>
                                <ColumnDefinition Width="Auto"/>
                            </Grid.ColumnDefinitions>
                            <TextBlock Style="{StaticResource SelectedStyle}"/>
                            <Path x:Name="Path" Grid.Column="1" Style="{StaticResource PathStyle}"/>
                        </Grid>
                    </Border>
                    <ControlTemplate.Triggers>
                        <Trigger Property="IsChecked" Value="True">
                            <Setter TargetName="Path" Property="Data" Value="{StaticResource CalendarSelectionGeometry}"/>
                        </Trigger>
                    </ControlTemplate.Triggers>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

## CalendarBox.xaml (ListBox)
_SmartDateControl/Thems/Units/CalendarBox.xaml_
```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:james="https://jamesnet.dev/xaml/presentation"
    xmlns:units="clr-namespace:SmartDateControl.UI.Units">

    <Style TargetType="{x:Type units:CalendarBox}">
        <Setter Property="SelectedValuePath" Value="DateFormat"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type units:CalendarBox}">
                    <ItemsPresenter/>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
        <Setter Property="ItemsPanel">
            <Setter.Value>
                <ItemsPanelTemplate>
                    <UniformGrid Columns="7"/>
                </ItemsPanelTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

## CalendarBoxItem.xaml (ListBoxItem)
_SmartDateControl/Themes/Units/CalendarBoxItem.xaml_
```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:units="clr-namespace:SmartDateControl.UI.Units">

    <Style TargetType="{x:Type TextBlock}" x:Key="DayStyle">
        <Setter Property="Text" Value="{Binding RelativeSource={RelativeSource TemplatedParent},Path=Content,StringFormat={}{0}}"/>
        <Setter Property="HorizontalAlignment" Value="Center"/>
        <Setter Property="VerticalAlignment" Value="Center"/>
    </Style>

    <Style TargetType="{x:Type units:CalendarBoxItem}">
        <Setter Property="Background" Value="Transparent"/>
        <Setter Property="Foreground" Value="#666666"/>
        <Setter Property="Width" Value="28"/>
        <Setter Property="Height" Value="{Binding RelativeSource={RelativeSource Self},Path=ActualWidth}"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type units:CalendarBoxItem}">
                    <Grid Background="Transparent">
                        <Border Background="{TemplateBinding Background}" Margin="4" CornerRadius="4"/>
                        <TextBlock Style="{StaticResource DayStyle}"/>
                    </Grid>
                    <ControlTemplate.Triggers>
                        <Trigger Property="IsCurrentMonth" Value="True">
                            <Setter Property="Foreground" Value="#FFFFFF"/>
                        </Trigger>
                        <Trigger Property="IsSelected" Value="True">
                            <Setter Property="Background" Value="#FFFFFF"/>
                            <Setter Property="Foreground" Value="#000000"/>
                        </Trigger>
                    </ControlTemplate.Triggers>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

## CalendarBoxItem.cs
_SmartDateControl.UI.Units.CalendarBoxItem_
```csharp
﻿using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace SmartDateControl.UI.Units
{
    public class CalendarBoxItem : ListBoxItem
    {
        public string DateFormat { get; set; }

        public DateTime Date;
        public bool IsCurrentMonth
        {
            get { return (bool)GetValue(IsCurrentMonthProperty); }
            set { SetValue(IsCurrentMonthProperty, value); }
        }

        public static readonly DependencyProperty IsCurrentMonthProperty =
            DependencyProperty.Register("IsCurrentMonth", typeof(bool), typeof(CalendarBoxItem), new PropertyMetadata(false));


        static CalendarBoxItem()
        {
            DefaultStyleKeyProperty.OverrideMetadata(typeof(CalendarBoxItem), new FrameworkPropertyMetadata(typeof(CalendarBoxItem)));
        }
    }
}
```

## ChevronButton.xaml (ToggleButton)
_SmartDateControl/Themes/Units/ChevronButon.xaml_
```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:james="https://jamesnet.dev/xaml/presentation"
    xmlns:units="clr-namespace:SmartDateControl.UI.Units">

    <Geometry x:Key="ChevronLeftGeometry">M15.41,16.58L10.83,12L15.41,7.41L14,6L8,12L14,18L15.41,16.58Z</Geometry>
    <Geometry x:Key="ChevronRightGeometry">M8.59,16.58L13.17,12L8.59,7.41L10,6L16,12L10,18L8.59,16.58Z</Geometry>

    <Style TargetType="{x:Type units:ChevronButton}">
        <Setter Property="VerticalAlignment" Value="Center"/>
        <Setter Property="Background" Value="Transparent"/>
        <Setter Property="Foreground" Value="#11FFFFFF"/>
        <Setter Property="Margin" Value="10 10 10 5"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type units:ChevronButton}">
                    <Grid Background="{TemplateBinding Background}">
                        <Path x:Name="Path" Fill="{TemplateBinding Foreground}" Width="12" Height="12" Stretch="Uniform"/>
                    </Grid>
                    <ControlTemplate.Triggers>
                        <Trigger Property="Tag" Value="Left">
                            <Setter TargetName="Path" Property="Data" Value="{StaticResource ChevronLeftGeometry}"/>
                        </Trigger>
                        <Trigger Property="Tag" Value="Right">
                            <Setter TargetName="Path" Property="Data" Value="{StaticResource ChevronRightGeometry}"/>
                        </Trigger>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter Property="Foreground" Value="#FFFFFF"/>
                            <Setter Property="Cursor" Value="Hand"/>
                        </Trigger>
                    </ControlTemplate.Triggers>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

## DayOfWeek.xaml (Label)
_SmartDateControl.UI.Units.DayOfWeek_
```xaml
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:units="clr-namespace:SmartDateControl.UI.Units">

    <Style TargetType="{x:Type units:DayOfWeek}">
        <Setter Property="FontSize" Value="11"/>
        <Setter Property="FontWeight" Value="Regular"/>
        <Setter Property="Foreground" Value="#999999"/>
        <Setter Property="Width" Value="28"/>
        <Setter Property="Height" Value="{Binding RelativeSource={RelativeSource Self},Path=ActualWidth}"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="{x:Type units:DayOfWeek}">
                    <Border Background="{TemplateBinding Background}">
                        <TextBlock Text="{TemplateBinding Content}" TextAlignment="Center" VerticalAlignment="Center"/>
                    </Border>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

## Communication and Support

We keep various channels open for communication at any time.

- [GitHub](https://github.com/vickyqu115/smartdate): Follow, Fork, Stars
- [YouTube](https://www.youtube.com/watch?v=oTtM5NNVCNc), [BiliBili](https://www.bilibili.com/video/BV1pE421L7c2): Like, Comment, Subscribe, Share
- [Facebook](https://facebook.com/jamesnet214), Email: vickyqu115@gmail.com, james@jamesnet.dev

## Original Tutorial Series
So far, a total of five episodes in the tutorial series have been released.

| No. | Platform | Title | YouTube | BiliBili |
|:---:|:----|:------|:-----:|:------:|
| 1 | WPF | Theme Switch | :link:[Link](https://www.youtube.com/watch?v=rGox76Bm6VY) | :link:[Link](https://www.bilibili.com/video/BV1ez4y1N7B8) |
| 2 | WPF | Riot PlayButton | :link:[Link](https://www.youtube.com/watch?v=xgUqDavCJGg) | :link:[Link](https://www.bilibili.com/video/BV1Tu4y1j7Ei) |
| 3 | WPF | Magic Navigation Bar | :link:[Link](https://www.youtube.com/watch?v=dxuLWlukthg) | :link:[Link](https://www.bilibili.com/video/BV1Ui4y1a717) |
| 4 | WPF | Riot Slider | :link:[Link](https://www.youtube.com/watch?v=jyv2fP9TUtY) | :link:[Link](https://www.bilibili.com/video/BV1uy421a7yM) |
| 5 | WPF | Smart Date | :link:[Link](https://www.youtube.com/watch?v=oTtM5NNVCNc) | :link:[Link](https://www.bilibili.com/video/BV1pE421L7c2) |

#### Preview of the Sixth Video

The upcoming sixth tutorial video will focus on dependency injection and CustomControl, incorporating project decentralization to implement a small WPF framework application.

## Contributor Introduction

Vicky Guo and James Lee are a developer couple active in South Korea and China, respectively. They handle everything from topic selection, sample source code preparation, recording the application implementation process, English/Chinese dubbing, Korean subtitling, video editing, thumbnail creation, and video uploads. Since they are full-time developers, uploads can sometimes be delayed. However, they find joy and a sense of mission in their work, making it a pleasure to continue.

## Ideas & Feedback

We are always waiting for new tutorial topics, ideas, and feedback!
vickyqu115@gmail.com,
james@jamesnet.dev
