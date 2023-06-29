# test
import plotly.graph_objects as go
from IPython.display import HTML

def average_animation(array):
    iterations = [0]
    data = [array.copy()]
    labels = [f'操作回数: {iterations[0]}']

    def average():
        for i in range(100):
            if i % 2 == 0:
                for j in range(len(array) // 2):
                    m = (array[2*j] + array[2*j + 1]) / 2
                    array[2*j:2*j + 2] = [m, m]
                if len(array) % 2 == 1:  # 配列の数が奇数個の場合、最後の要素を最初の要素と平準化する
                    array[-1] = array[0]
            else:
                for j in range(len(array) // 2):
                    m = (array[2*j + 1] + array[(2*j + 2) % len(array)]) / 2
                    array[2*j:2*j + 2] = [m, m]

            data.append(array.copy())
            iterations[0] += 1
            labels.append(f'操作回数: {iterations[0]}')

    average()

    frames = []
    for i in range(len(data)):
        frame = go.Frame(
            data=[go.Bar(x=list(range(1, len(data[0]) + 1)), y=data[i])],
            layout=go.Layout(title=labels[i])
        )
        frames.append(frame)

    fig = go.Figure(
        data=[go.Bar(x=list(range(1, len(data[0]) + 1)), y=data[0])],
        layout=go.Layout(title=labels[0], yaxis=dict(range=[0, 500])),
        frames=frames
    )

    animation = fig.to_html(full_html=False)
    return animation

my_array = [350, 450, 40, 160, 370, 130, 120, 180]
animation = average_animation(my_array)
HTML(animation)
