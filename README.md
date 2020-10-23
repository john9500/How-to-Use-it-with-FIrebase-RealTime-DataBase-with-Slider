# How-to-Use-it-with-FIrebase-RealTime-DataBase-with-Slider
#Retrive Database
db.collection(location)
            .get()
            .addOnCompleteListener(new OnCompleteListener<QuerySnapshot>() {
                @Override
                public void onComplete(@NonNull Task<QuerySnapshot> task) {
                    if (task.isSuccessful()) {
                        for (QueryDocumentSnapshot document : task.getResult()) {
                            Slider slider = document.toObject(Slider.class);
                            sliderArrayList.add(slider);
                        }

                        sliderV.setSliderAdapter(new SliderAdapter(getActivity(),sliderArrayList));
                        sliderV.startAutoCycle();
                        sliderV.setIndicatorAnimation(IndicatorAnimations.WORM);
                        sliderV.setSliderTransformAnimation(SliderAnimations.SIMPLETRANSFORMATION);

                    } else {
                        Log.w("FIREBASE", "Error getting documents.", task.getException());
                    }
                }
            });`
            
            #In Adapter Class
            
            Glide.with(viewHolder.itemView) .load(sliderArrayList.get(position).getImageUrl()) .into(viewHolder.imageViewBackground);
            
#SliderAdapter.java

public class SliderAdapter extends SliderViewAdapter<SliderAdapter.SliderAdapterVH> {

Context context;
List<Slider> m;

SliderAdapter(List<Slider> m, Context context) {
    this.m = m;
    this.context = context;
}

@Override
public SliderAdapterVH onCreateViewHolder(ViewGroup parent) {
    View inflate = LayoutInflater.from(parent.getContext()).inflate(R.layout.image_slider_layout_item, null);
    return new SliderAdapterVH(inflate);
}

@Override
public void onBindViewHolder(SliderAdapterVH viewHolder, int position) {

    final Slider imgFeed = m.get(position);
    Glide.with(viewHolder.itemView).load(imgFeed.getImg()).into(viewHolder.imageViewBackground);
    viewHolder.textViewDescription.setText(imgFeed.getTxt());
}

@Override
public int getCount() {
    return m.size();
}

class SliderAdapterVH extends SliderViewAdapter.ViewHolder {

    View itemView;
    ImageView imageViewBackground;
    TextView textViewDescription;

    public SliderAdapterVH(View itemView) {
        super(itemView);
        imageViewBackground = itemView.findViewById(R.id.iv_auto_image_slider);
        textViewDescription = itemView.findViewById(R.id.tv_auto_image_slider);
        this.itemView = itemView;
    }
}
}
