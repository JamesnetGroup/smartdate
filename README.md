# smartdate
![image](https://github.com/vickyqu115/smartdate/assets/101777355/e2077f6e-552f-4cb9-ae07-deb350c66d80)
## Content

- Tutorial Introduction
- Overview
- WPF SmartDate
- Contributor Introduction
- Communication and Support
- Tutorial Series
- Ideas & Feedback

<br/>
![](https://jamesnetdev.blob.core.windows.net/articleimages/9ca4ff54-3ac2-4e2b-a336-26aacb0df457.png)

## Tutorial Introduction
The WPF SmartDate tutorial is provided through videos and a GitHub repository with source code, and we strive to deliver detailed and extensive learning content and updates through Code Project, community channels, Facebook, and blogs.

- [YouTube](https://bit.ly/3xOeyMJ): English audio, Korean subtitles
- [BiliBili](https://bit.ly/3xI9DNh): Chinese audio
- [GitHub](https://github.com/vickyqu115/smartdate): Source code, includes README.md
- [Code-Project](https://www.codeproject.com/search.aspx?q=vickyqu115&x=0&y=0&sbo=kw): Project review pending.

## Overview

This video marks the fifth installment of the WPF tutorial series, starting with ThemeSwitch. We've mainly covered content derived from ContentControl and ItemsControl such as Buttons, Sliders, and ListBoxes. However, this time we delve into more complex functionalities like those in a DatePicker. Therefore, this series will be more challenging than previous ones.

But there's no need to worry. This tutorial video is prepared with care to ensure you can learn through repetition, with all processes, contents, and explanations arranged in an orderly fashion. Just repeat until you become familiar and proficient.
## WPF SmartDate

For nearly 20 years since its inception, most controls derived from Template-including Control, ContentControl, and ItemsControl in WPF have been widely used without significant lack in classes, interfaces, and provided Dependency Property attributes. This shows how precisely WPF is designed. Especially, WPF's Template and hierarchy-focused design structure suit most controls like Buttons, ComboBoxes, and ListBoxes well.

However, controls like DatePicker, which demand complex functionalities and detailed customization, have made us feel the limitations of the basic controls provided. In contrast to [Riot Slider](https://bit.ly/3xUkIv2) from a previous tutorial, which featured a monotonous function and simple Template structure making it meaningful to analyze and customize the Template structure, DatePicker operates almost like a small application with various internal processes, making it a very challenging task to extract and analyze the basic Template structure. This will serve as an excellent training in WPF research and analysis.

If you are looking to analyze and study the basic control setup of DatePicker, this is a great approach. Additionally, we recommend examining the SmartDate control method featured in this tutorial. The main processes and content of this video are as follows:

| Order | Main Task | Summary of Main Content |
|:--:|:---|:---|
| 1 | WPF Application | Creation of program entry structure and creation and execution of Application instance Run() |
| 2 | WPF Class Library | Basic structure of CustomControl: AssemblyInfo.cs, Generic.xaml, CustomControl |
| 3 | SmartDate | CustomControl derived from replacing the DatePicker |
| 6 | CalendarSwitch | CustomControl derived from a ToggleButton that manages the popup switching of the SmartDate control |
| 7 | Popup | Basic control encompassing the calendar |
| 4 | CalendarBox | CustomControl derived from a ListBox displaying dates on the calendar, specifying ItemsPresenter's container as a UniformGrid via ItemsPanelTemplate, including moving buttons for the calendar in the ControlTemplate |
| 5 | CalendarBoxItem | CustomControl derived from ListBoxItem which acts as the ItemsContainer for the CalendarBox |
| 7 | ChevronButton | CustomControl derived from a Button that moves to the previous or next month |
| 8 | DayOfWeek | CustomControl derived from a Label that displays the days of the week |

A look at the main tasks shows that not only SmartDate but also its subordinate controls utilize CustomControl. This design provides a good example of the philosophy behind WPF's CustomControl design.

Through this tutorial video, we hope you gain a detailed understanding of the implementation of CustomControl. If you need more preliminary study on the concept of Template, we recommend watching the RiotSlider tutorial video first.

## Communication and Support

We keep various channels open for communication at any time.

- [GitHub](https://github.com/vickyqu115/smartdate): Follow, Fork, Stars
- [YouTube](https://bit.ly/3xOeyMJ), [BiliBili](https://bit.ly/3xI9DNh): Like, Comment, Subscribe, Share
- [Facebook](https://facebook.com/jamesnet214), Email: james@jamesnet.dev

net.dev

## Original Tutorial Series
So far, a total of five episodes in the tutorial series have been released.

| No. | Platform | Title | YouTube | BiliBili |
|:---:|:----|:------|:-----:|:------:|
| 1 | WPF | Theme Switch | :link:[Link](https://bit.ly/3uBkFlQ) | :link:[Link](https://bit.ly/3uHFe08) |
| 2 | WPF | Riot PlayButton | :link:[Link](https://bit.ly/40YoVIo) | :link:[Link](https://bit.ly/49L6dXu) |
| 3 | WPF | Magic Navigation Bar | :link:[Link](https://bit.ly/3TVeRhF) | :link:[Link](https://bit.ly/3UvaOsl) |
| 4 | WPF | Riot Slider | :link:[Link](https://bit.ly/3xUkIv2) | :link:[Link](https://bit.ly/3QiZvkJ) |
| 5 | WPF | Smart Date | :link:[Link](https://bit.ly/3xOeyMJ) | :link:[Link](https://bit.ly/3xI9DNh) |


